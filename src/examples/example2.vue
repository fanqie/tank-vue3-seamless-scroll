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
        <div v-for="i in 6" :key="i" class="demo-item">test{{ i }}</div>
      </tank-seamless-scroll>
    </div>
    <div v-else>
      <code-show :code="code" />
    </div>
  </div>

</template>
<script setup>
import {ref} from 'vue'
import TankSeamlessScroll from "../Index.vue"
import CodeShow from "../components/codeShow.vue"

const prop = defineProps({
  stepLength: {
    type: Number,
    default: 260,
  },
  reverse: {
    type: Boolean,
    default: false
  },
  debug: {
    type: Boolean,
    default: false
  },
  height: {
    type: Number,
    default: 200
  },
  title: {
    type: String,
    default: ""
  }

})
const count =  ref(new Array(5).fill(null))
const demoTab = ref('preview')
const code = `<tank-seamless-scroll :step-length="15" :debug="true" :reverse="false">
  <div v-for=\"i in 6\" :key=\"i\" class=\"demo-item\">test{{ i }}</div>
</tank-seamless-scroll>`
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
  background: linear-gradient(135deg, #238636 0%, #2ea043 100%);
  margin: 4px 0;
  box-sizing: border-box;
  text-align: center;
  border-radius: 6px;
  font-size: 13px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.demo .demo-item:nth-child(2n) {
  background: linear-gradient(135deg, #1a7f37 0%, #238636 100%);
}
</style>
