<template>
  <div class="complex-item" :class="{ loaded: isLoaded }">
    <div class="avatar" :style="{ backgroundColor: avatarColor }">
      {{ initials }}
    </div>
    <div class="content">
      <div class="title">{{ item.name }}</div>
      <div class="meta">
        <span class="badge" :style="{ backgroundColor: statusColor }">{{ item.status }}</span>
        <span class="time">{{ formattedTime }}</span>
        <span class="counter">renders: {{ renderCount }}</span>
      </div>
      <div class="progress-bar">
        <div class="progress" :style="{ width: `${progress}%` }"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, watch } from 'vue'

const props = defineProps({
  item: { type: Object, required: true },
  index: { type: Number, required: true }
})

// 模拟复杂状态
const isLoaded = ref(false)
const progress = ref(0)
const renderCount = ref(0)
let animationTimer = null
let progressTimer = null

// 计算属性
const initials = computed(() => {
  return props.item.name?.substring(0, 2) || '??'
})

const avatarColor = computed(() => {
  const colors = ['#1f6feb', '#238636', '#da3633', '#9e6a03', '#8b949e']
  return colors[props.index % colors.length]
})

const statusColor = computed(() => {
  const map = { active: '#238636', pending: '#9e6a03', inactive: '#da3633' }
  return map[props.item.status] || '#8b949e'
})

const formattedTime = computed(() => {
  return new Date(props.item.timestamp).toLocaleTimeString()
})

// 生命周期 - 模拟复杂初始化
onMounted(() => {
  renderCount.value++
  
  // 模拟异步加载
  animationTimer = setTimeout(() => {
    isLoaded.value = true
  }, 50)
  
  // 模拟进度动画
  progressTimer = setInterval(() => {
    progress.value = (progress.value + 10) % 100
  }, 500)
})

onUnmounted(() => {
  // 清理定时器
  if (animationTimer) clearTimeout(animationTimer)
  if (progressTimer) clearInterval(progressTimer)
})

// 监听props变化
watch(() => props.item, () => {
  renderCount.value++
}, { deep: true })
</script>

<style scoped>
.complex-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 12px;
  background: #21262d;
  border-bottom: 1px solid #30363d;
  opacity: 0.5;
  transition: opacity 0.2s ease;
}

.complex-item.loaded {
  opacity: 1;
}

.avatar {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  font-size: 12px;
  font-weight: 600;
  flex-shrink: 0;
}

.content {
  flex: 1;
  min-width: 0;
}

.title {
  color: #e6edf3;
  font-size: 13px;
  font-weight: 500;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.meta {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 4px;
  font-size: 11px;
  color: #8b949e;
}

.badge {
  padding: 1px 6px;
  border-radius: 10px;
  color: #fff;
  font-size: 10px;
}

.counter {
  color: #f85149;
}

.progress-bar {
  height: 2px;
  background: #30363d;
  border-radius: 1px;
  margin-top: 6px;
  overflow: hidden;
}

.progress {
  height: 100%;
  background: linear-gradient(90deg, #1f6feb, #58a6ff);
  transition: width 0.3s ease;
}
</style>
