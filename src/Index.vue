<template>
  <div 
    ref="ref_tank_seamless_scroll" 
    class="tank_seamless_scroll" 
    :style="containerStyle"
    @mouseenter="handleMouseEnter"
    @mouseleave="handleMouseLeave"
  >
    <div class="debugger" v-if="prop.debug">
      <div>speed:{{ prop.stepLength }}px/s | duration:{{ animationDuration.toFixed(2) }}s</div>
      <div>dir:{{ prop.reverse ? 'reverse' : 'normal' }} | status:{{ isPaused ? 'paused' : 'running' }}</div>
      <div>contentH:{{ contentHeight }}px | fps:{{ currentFps }}</div>
      <div v-if="prop.virtual">visible:{{ visibleRange.start }}-{{ visibleRange.end }}/{{ prop.data.length }}</div>
      <div v-else>copies:{{ copyCount }}</div>
    </div>
    
    <!-- 虚拟滚动模式 -->
    <div 
      v-if="prop.virtual"
      ref="ref_warp" 
      class="warp"
      :class="wrapClass"
      :style="virtualWrapStyle"
    >
      <!-- 顶部占位 -->
      <div :style="{ height: `${virtualPaddingTop}px` }"></div>
      <!-- 可见区域项目 -->
      <div 
        v-for="item in visibleItems" 
        :key="item._virtualKey" 
        class="warpLine virtual-item"
        :style="{ height: `${prop.itemHeight}px` }"
      >
        <slot name="item" :item="item._data" :index="item._index"></slot>
      </div>
      <!-- 底部占位 -->
      <div :style="{ height: `${virtualPaddingBottom}px` }"></div>
    </div>
    
    <!-- 普通模式 -->
    <div 
      v-else
      ref="ref_warp" 
      class="warp"
      :class="wrapClass"
      :style="wrapStyle"
    >
      <div 
        v-for="i in copyCount" 
        :key="i" 
        class="warpLine" 
        :ref="el => { if (i === 1) ref_warpLine = el }"
        :aria-hidden="i > 1"
        :style="{ 'contain-intrinsic-size': `auto ${contentHeight}px` }"
      >
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted, nextTick, ref, computed, watch } from "vue";

const prop = defineProps({
  stepLength: {
    type: Number,
    default: 60,
    validator: (value) => value >= 0,
  },
  reverse: {
    type: Boolean,
    default: false
  },
  debug: {
    type: Boolean,
    default: false
  },
  pauseOnHover: {
    type: Boolean,
    default: true
  },
  // 虚拟滚动模式
  virtual: {
    type: Boolean,
    default: false
  },
  // 虚拟模式数据
  data: {
    type: Array,
    default: () => []
  },
  // 每项高度（虚拟模式必须）
  itemHeight: {
    type: Number,
    default: 50
  },
  // 缓冲区项目数
  buffer: {
    type: Number,
    default: 5
  }
})

// Refs
const ref_tank_seamless_scroll = ref(null)
const ref_warpLine = ref(null)
const ref_warp = ref(null)

// State
const parentHeight = ref(0)
const contentHeight = ref(0)
const animationDuration = ref(0)
const isPaused = ref(false)
const currentFps = ref(60)
const isReady = ref(false)
const copyCount = ref(2)

// 虚拟滚动状态
const scrollOffset = ref(0)
const visibleRange = ref({ start: 0, end: 0 })
let animationStartTime = 0
let virtualRafId = null
let pausedOffset = 0 // 暂停时保存的位置

// FPS 监控
let fpsFrameCount = 0
let fpsLastTime = performance.now()
let fpsRafId = null

const updateFps = () => {
  fpsFrameCount++
  const now = performance.now()
  const elapsed = now - fpsLastTime
  
  if (elapsed >= 1000) {
    currentFps.value = Math.round(fpsFrameCount * 1000 / elapsed)
    fpsFrameCount = 0
    fpsLastTime = now
  }
  
  fpsRafId = requestAnimationFrame(updateFps)
}

// Computed styles
const containerStyle = computed(() => ({
  height: parentHeight.value ? `${parentHeight.value}px` : '100%'
}))

const wrapClass = computed(() => {
  // 虚拟模式不使用CSS动画，用JS控制
  if (prop.virtual) {
    return {
      'warp-paused': isPaused.value && prop.pauseOnHover
    }
  }
  return {
    'warp-animated': isReady.value,
    'warp-paused': isPaused.value && prop.pauseOnHover,
    'warp-reverse': prop.reverse
  }
})

const wrapStyle = computed(() => {
  if (!contentHeight.value || !animationDuration.value) return {}
  
  return {
    '--scroll-duration': `${animationDuration.value}s`,
    '--scroll-distance': `${contentHeight.value}px`,
  }
})

// 虚拟滚动样式
const virtualTotalHeight = computed(() => {
  if (!prop.virtual) return 0
  return prop.data.length * prop.itemHeight
})

const virtualWrapStyle = computed(() => {
  if (!prop.virtual) return {}
  
  // 虚拟模式通过JS控制transform
  return {
    transform: `translate3d(0, ${-scrollOffset.value}px, 0)`,
    willChange: 'transform'
  }
})

const virtualPaddingTop = computed(() => {
  return visibleRange.value.start * prop.itemHeight
})

const virtualPaddingBottom = computed(() => {
  const totalItems = prop.data.length * 2 // 两份副本
  return Math.max(0, (totalItems - visibleRange.value.end) * prop.itemHeight)
})

const visibleItems = computed(() => {
  if (!prop.virtual || !prop.data.length) return []
  
  const { start, end } = visibleRange.value
  const items = []
  const dataLen = prop.data.length
  
  for (let i = start; i < end; i++) {
    const realIndex = i % dataLen
    items.push({
      _virtualKey: `${i}-${realIndex}`,
      _index: realIndex,
      _data: prop.data[realIndex]
    })
  }
  
  return items
})

// Calculate animation duration based on stepLength
const calculateDuration = () => {
  if (prop.virtual) {
    if (!virtualTotalHeight.value || prop.stepLength === 0) return 0
    return virtualTotalHeight.value / prop.stepLength
  } else {
    if (!contentHeight.value || prop.stepLength === 0) return 0
    return contentHeight.value / prop.stepLength
  }
}

// 虚拟滚动：更新可见范围
const updateVisibleRange = () => {
  if (!prop.virtual) return
  // 如果尚未测得高度或没有数据/时长，稍后再尝试
  if (!parentHeight.value || !prop.data.length || !animationDuration.value) {
    virtualRafId = requestAnimationFrame(updateVisibleRange)
    return
  }
  if (isPaused.value && prop.pauseOnHover) {
    virtualRafId = requestAnimationFrame(updateVisibleRange)
    return
  }
  
  const totalHeight = virtualTotalHeight.value
  if (totalHeight <= 0) return
  
  const elapsed = (performance.now() - animationStartTime) / 1000
  const cycleDuration = animationDuration.value || 1
  let progress = (elapsed % cycleDuration) / cycleDuration
  
  // 计算当前滚动位置
  let offset = progress * totalHeight
  if (prop.reverse) {
    offset = totalHeight - offset
  }
  scrollOffset.value = offset
  
  // 计算可见范围
  const startIndex = Math.floor(offset / prop.itemHeight)
  const visibleCount = Math.ceil(parentHeight.value / prop.itemHeight)
  const endIndex = startIndex + visibleCount + prop.buffer * 2
  
  visibleRange.value = {
    start: Math.max(0, startIndex - prop.buffer),
    end: Math.min(prop.data.length * 2, endIndex)
  }
  
  virtualRafId = requestAnimationFrame(updateVisibleRange)
}

// Update measurements
const updateMeasurements = () => {
  if (!ref_tank_seamless_scroll.value) return
  
  const parentEl = ref_tank_seamless_scroll.value.parentElement
  if (!parentEl) return
  
  const pHeight = parentEl.getBoundingClientRect().height
  parentHeight.value = pHeight
  
  if (prop.virtual) {
    // 虚拟模式
    contentHeight.value = virtualTotalHeight.value
    animationDuration.value = calculateDuration()
    
    // 初始化可见范围
    const visibleCount = Math.ceil(pHeight / prop.itemHeight)
    visibleRange.value = {
      start: 0,
      end: Math.min(prop.data.length * 2, visibleCount + prop.buffer * 2)
    }
    
    isReady.value = prop.data.length > 0
  } else {
    // 普通模式
    if (!ref_warpLine.value) return
    
    const singleHeight = ref_warpLine.value.offsetHeight
    
    if (singleHeight > 0) {
      contentHeight.value = singleHeight
      const needed = Math.ceil(pHeight / singleHeight) + 1
      copyCount.value = Math.max(2, needed)
      animationDuration.value = calculateDuration()
      isReady.value = true
    }
  }
}

// Mouse event handlers
const handleMouseEnter = () => {
  if (prop.pauseOnHover) {
    isPaused.value = true
    if (prop.virtual) {
      // 保存当前位置
      pausedOffset = scrollOffset.value
    }
    if (virtualRafId) {
      cancelAnimationFrame(virtualRafId)
      virtualRafId = null
    }
  }
}

const handleMouseLeave = () => {
  if (prop.pauseOnHover) {
    isPaused.value = false
    if (prop.virtual && prop.data.length > 0) {
      // 从保存的位置继续，避免跳动
      const totalHeight = virtualTotalHeight.value
      if (totalHeight > 0) {
        let progress = pausedOffset / totalHeight
        // 反向滚动时，进度需要反转
        if (prop.reverse) {
          progress = 1 - progress
        }
        animationStartTime = performance.now() - progress * animationDuration.value * 1000
      }
      virtualRafId = requestAnimationFrame(updateVisibleRange)
    }
  }
}

// Watch for prop changes
watch(() => prop.stepLength, () => {
  animationDuration.value = calculateDuration()
})

watch(() => prop.data, () => {
  if (prop.virtual) {
    updateMeasurements()
    // 重新启动循环，确保数据就绪后自动滚动
    if (virtualRafId) {
      cancelAnimationFrame(virtualRafId)
      virtualRafId = null
    }
    if (prop.data.length > 0) {
      animationStartTime = performance.now()
      virtualRafId = requestAnimationFrame(updateVisibleRange)
    }
  }
}, { deep: true })

// Lifecycle hooks
let resizeObserver = null

onMounted(() => {
  nextTick(() => {
    updateMeasurements()
    
    // 启动 FPS 监控
    if (prop.debug) {
      fpsRafId = requestAnimationFrame(updateFps)
    }
    
    // 虚拟模式启动位置追踪
    if (prop.virtual && prop.data.length > 0) {
      animationStartTime = performance.now()
      virtualRafId = requestAnimationFrame(updateVisibleRange)
    } else if (prop.virtual) {
      // 数据尚未就绪时，保持循环等待，避免“无滚动”感知
      virtualRafId = requestAnimationFrame(updateVisibleRange)
    }
    
    // Watch for size changes
    if (window.ResizeObserver) {
      resizeObserver = new ResizeObserver(() => {
        updateMeasurements()
      })
      
      if (ref_tank_seamless_scroll.value?.parentElement) {
        resizeObserver.observe(ref_tank_seamless_scroll.value.parentElement)
      }
      if (!prop.virtual && ref_warpLine.value) {
        resizeObserver.observe(ref_warpLine.value)
      }
    }
  })
})

onUnmounted(() => {
  if (resizeObserver) {
    resizeObserver.disconnect()
  }
  if (fpsRafId) {
    cancelAnimationFrame(fpsRafId)
  }
  if (virtualRafId) {
    cancelAnimationFrame(virtualRafId)
  }
})
</script>

<style scoped>
.tank_seamless_scroll {
  position: relative;
  overflow: hidden;
  width: 100%;
}

.warp {
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
}

/* 关键：使用CSS动画实现无缝滚动 */
.warp-animated {
  animation-name: seamless-scroll;
  animation-duration: var(--scroll-duration, 10s);
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  animation-play-state: running;
}

.warp-paused {
  animation-play-state: paused !important;
}

.warp-reverse {
  animation-direction: reverse;
}

/* 核心关键帧 */
@keyframes seamless-scroll {
  0% {
    transform: translate3d(0, 0, 0);
  }
  100% {
    transform: translate3d(0, calc(var(--scroll-distance) * -1), 0);
  }
}

.warpLine {
  padding: 0;
  margin: 0;
  flex-shrink: 0;
  content-visibility: auto;
}

.virtual-item {
  box-sizing: border-box;
}

.debugger {
  position: absolute;
  top: 5px;
  left: 5%;
  width: 90%;
  height: auto;
  padding: 8px 12px;
  background-color: rgba(0, 0, 0, .85);
  color: #aaa;
  z-index: 88;
  border-radius: 8px;
  border: solid 1px #666;
  box-sizing: border-box;
  font-size: 12px;
  line-height: 1.6;
}

.debugger div {
  white-space: nowrap;
}
</style>
