<template>
  <div class="demo">
    <h2 v-if="title">{{title}}</h2>
    <ul class="nav nav-tabs small-tabs">
      <li class="nav-item">
        <a href="#" class="nav-link" :class="{ active: demoTab==='preview' }" @click.prevent="demoTab='preview'">Preview</a>
      </li>
      <li class="nav-item">
        <a href="#" class="nav-link" :class="{ active: demoTab==='code' }" @click.prevent="demoTab='code'">Code</a>
      </li>
    </ul>
    <div v-if="demoTab==='preview'" class="preview-area" :style="{height:`${height}px`}">
      <tank-seamless-scroll :step-length="stepLength" :debug="debug" :reverse="reverse">
        <div v-for="(v,i) in count" :key="i" class="demo-item">value:{{ v }},key:{{ i }}</div>
      </tank-seamless-scroll>
    </div>
    <div v-else>
      <code-show :code="code" />
    </div>
  </div>
        
</template>
<script setup>
import {ref, onMounted, onUnmounted} from 'vue'
import TankSeamlessScroll from "../Index.vue"
import CodeShow from "../components/codeShow.vue"
const code=`<tank-seamless-scroll :step-length=\"stepLength\" :debug=\"debug\" :reverse=\"reverse\">\n  <div v-for=\"(v,i) in count\" :key=\"i\" class=\"demo-item\">value:{{ v }},key:{{ i }}</div>\n</tank-seamless-scroll>`
const prop = defineProps({
  stepLength: {
    type: Number,
    default: 260,
  },
  reverse: {
    type: Boolean,
    default: false
  },
  height: {
    type: Number,
    default: 200
  },
  debug: {
    type: Boolean,
    default: false
  },
  title: {
    type: String,
    default: ""
  }

})
const count = ref(new Array(10).fill(null))
const timer = 0
const demoTab = ref('preview')
onMounted(() => {
  setInterval(() => {
    //append data
    count.value = count.value.concat([null, null, null])
  }, 5000)
})
onUnmounted(() => {
  clearInterval(timer)
})
</script>
<style scoped>
h2 {
  color: var(--text-secondary, #8b949e);
  text-align: center;
  font-size: 14px;
  font-weight: 500;
  margin: 0 0 12px 0;
}
.demo {
  color: #fff;
}

.preview-area { margin-top: 8px; }

.small-tabs { margin-top: 6px; }
.small-tabs .nav-link { color: var(--text-secondary, #8b949e); background: transparent; border-color: var(--border-color, #30363d); }
.small-tabs .nav-link.active { color: #fff; background: #21262d; border-color: var(--border-color, #30363d); }

.demo .demo-item {
  padding: 12px 0;
  background: linear-gradient(135deg, #da3633 0%, #f85149 100%);
  margin: 4px 0;
  box-sizing: border-box;
  text-align: center;
  border-radius: 6px;
  font-size: 13px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.demo .demo-item:nth-child(2n) {
  background: linear-gradient(135deg, #b62324 0%, #da3633 100%);
}
</style>
