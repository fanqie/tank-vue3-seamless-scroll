<template>
  <div class="demo">
    <h2 v-if="title">{{title}} ({{ listData.length }} items)</h2>
    <ul class="nav nav-tabs small-tabs">
      <li class="nav-item">
        <a href="#" class="nav-link" :class="{ active: demoTab==='preview' }" @click.prevent="demoTab='preview'">Preview</a>
      </li>
      <li class="nav-item">
        <a href="#" class="nav-link" :class="{ active: demoTab==='code' }" @click.prevent="demoTab='code'">Code</a>
      </li>
    </ul>
    <div v-if="demoTab==='preview'" class="preview-area" :style="{height:`${height}px`}">
      <tank-seamless-scroll 
        :step-length="stepLength" 
        :debug="debug" 
        :reverse="reverse"
        :pause-on-hover="true"
        :virtual="true"
        :data="listData"
        :item-height="40"
        :buffer="5"
      >
        <template #item="{ item, index }">
          <div class="virtual-row demo-item">
            {{ item.text }} (index: {{ index }})
          </div>
        </template>
      </tank-seamless-scroll>
    </div>
    <div v-else>
      <code-show :code="code" />
    </div>
  </div>
</template>

<script setup>
import { ref ,onMounted} from 'vue'
import TankSeamlessScroll from "../Index.vue"
import CodeShow from "../components/codeShow.vue"

const prop = defineProps({
  stepLength: {
    type: Number,
    default: 400,
  },
  reverse: {
    type: Boolean,
    default: false
  },
  height: {
    type: Number,
    default: 300
  },
  debug: {
    type: Boolean,
    default: false
  },
  title: {
    type: String,
    default: ""
  },
  count: {
    type: Number,
    default: 10000
  }
})
onMounted(() => {
   listData.value = 
  Array.from({ length: prop.count }, (_, i) => ({
    id: i,
    text: `Item #${i + 1}`
  }))

})
// 生成大量数据
let listData = ref([])
const demoTab = ref('preview')
const code = `<tank-seamless-scroll\n  :step-length=\"1000\"\n  :debug=\"true\"\n  :reverse=\"false\"\n  :pause-on-hover=\"true\"\n  :virtual=\"true\"\n  :data=\"listData\"\n  :item-height=\"40\"\n  :buffer=\"5\"\n>\n  <template #item=\"{ item, index }\">\n    <div class=\"virtual-row demo-item\">\n      {{ item.text }} (index: {{ index }})\n    </div>\n  </template>\n</tank-seamless-scroll>`
</script>

<style scoped>
h2 {
  color: var(--text-secondary, #8b949e);
  text-align: center;
  font-size: 14px;
  font-weight: 500;
  margin: 0 0 12px 0;
}
.demo { color: #fff; }
.preview-area { margin-top: 8px; }
.small-tabs { margin-top: 6px; }
.small-tabs .nav-link { color: var(--text-secondary, #8b949e); background: transparent; border-color: var(--border-color, #30363d); }
.small-tabs .nav-link.active { color: #fff; background: #21262d; border-color: var(--border-color, #30363d); }
.virtual-row {
  height: 40px;
  line-height: 40px;
  padding: 0 15px;
  box-sizing: border-box;
  background: linear-gradient(135deg, #1f6feb 0%, #388bfd 100%);
  border-bottom: 1px solid rgba(255,255,255,0.1);
  font-size: 13px;
}
.virtual-row:nth-child(2n) {
  background: linear-gradient(135deg, #1158c7 0%, #1f6feb 100%);
}
</style>
