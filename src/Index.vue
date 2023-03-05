<template>
  <div ref="ref_tank_seamless_scroll" class="tank_seamless_scroll" :style="{height: parentHeight+'px'}">
    <div class="debugger" v-if="debug">
      copyCount:{{ loopCount.length }}
      <br/>
      translateY:{{ Math.round(val * 1000) / 1000 }}px
      <br/>
      fps:{{ currentFps }}
    </div>
    <div ref="ref_warp" class="warp">
      <template v-for="i in loopCount.length" :key="'ss_'+i">
        <div ref="ref_warpLine" >
          <slot name="default"></slot>
        </div>
      </template>
    </div>
  </div>
</template>

<script setup>
import {onMounted, onUnmounted, nextTick, getCurrentInstance, ref} from "vue";
import {timer} from "d3-timer";

const loopCount = ref(new Array(5))
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
  limitFps: {
    type: Number,
    default: 32
  },
})
let t = null
const ref_tank_seamless_scroll = ref(null)
const ref_warpLine = ref(null)
const ref_warp = ref(null)
let parentHeight = ref(0)
const createTimer = (handle) => new Promise((resolve, reject) => {
  resolve(timer(() => {
    try {
      handle()
    } catch (err) {
      reject(err)
    }
  },1000/32))
})
const getTime = () => new Date().getTime()
const val = ref()
const currentFps = ref(64)
onMounted(() => {

      nextTick(() => {
        parentHeight.value = ref_tank_seamless_scroll.value.parentElement.clientHeight

        let lastTime = getTime()

        const warpLineHeight = ref_warpLine.value[0].clientHeight;
        let lastY = -warpLineHeight;
        let translateY = -warpLineHeight
        loopCount.value = new Array(Math.ceil(parentHeight.value / warpLineHeight) * 3)
        createTimer(() => {
          const limitHeight = prop.reverse ? -parentHeight.value * 0.5 : -parentHeight.value*2
          const currentTime = getTime()
          currentFps.value = Math.ceil(1000 / (currentTime - lastTime))
          const len = (currentTime - lastTime) * (prop.stepLength / 1000)
          lastTime = currentTime
          translateY += (prop.reverse ? 1 : -1) * len
          if (prop.reverse) {
            translateY = translateY > limitHeight ? -(Math.ceil(parentHeight.value / warpLineHeight) * warpLineHeight + Math.abs(translateY % warpLineHeight)) : translateY

          } else {
            translateY = translateY < limitHeight ?
                Math.sign(translateY) * (Math.abs(translateY % warpLineHeight) + warpLineHeight) :
                translateY
          }
          val.value = translateY
          ref_warp.value.style.setProperty("transform", `translate3d(0px ,${translateY}px,0px)`)
          lastY = translateY
        }).then((timer) => {
          t = timer
        }).catch(err => {
          console.error(err)
        })
      })
    }
)
onUnmounted(() => {
  t?.stop()
})
</script>

<style scoped>
.tank_seamless_scroll {
  position: relative;
  overflow: hidden;
}

.warp {
  transition: 0s;
}

.debugger {
  position: relative;
  top: 5px;
  left: 10.5%;
  width: 75%;
  height: auto;
  padding: 5px 2.5%;
  background-color: rgba(0, 0, 0, .8);
  color: #999999;
  z-index: 88;
  border-radius: 10px;
  border: solid 1px #999999;
}

</style>
