<template>
  <div class="demo">
    <h2 v-if="title">{{ title }}</h2>
    <ul class="nav nav-tabs small-tabs">
      <li class="nav-item">
        <a href="#" class="nav-link" :class="{ active: demoTab === 'preview' }" @click.prevent="demoTab = 'preview'">Preview</a>
      </li>
      <li class="nav-item">
        <a href="#" class="nav-link" :class="{ active: demoTab === 'code' }" @click.prevent="demoTab = 'code'">Code</a>
      </li>
    </ul>
    <div class="controls">
      <label>
        Threshold: <input type="number" v-model.number="threshold" min="0" max="20" />
      </label>
      <span class="hint" :style="{color: count.length>=threshold?'green':'orange'}">Items: {{ count.length }} | Scrolling: {{ count.length >= threshold ? 'Yes' : 'No' }}</span>
    </div>
    <div class="button-group">
      <button @click="addItem" class="btn">Add Item</button>
      <button @click="removeItem" class="btn">Remove Item</button>
      <button @click="resetItems" class="btn">Reset (3 items)</button>
    </div>
    <div v-if="demoTab === 'preview'" class="preview-area" :style="{ height: `${height}px` }">
      <tank-seamless-scroll :step-length="stepLength"  :auto-scroll-threshold="threshold">
        <div v-for="(v, i) in count" :key="i" class="demo-item">Item {{ i + 1 }}</div>
      </tank-seamless-scroll>
    </div>
    <div v-else>
      <code-show :code="code" />
    </div>
  </div>
</template>
<script setup>
import { ref, computed } from 'vue'
import TankSeamlessScroll from "../Index.vue"
import CodeShow from "../components/codeShow.vue"

const prop = defineProps({
  stepLength: {
    type: Number,
    default: 100,
  },
  debug: {
    type: Boolean,
    default: true
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

const count = ref(new Array(3).fill(null))
const threshold = ref(5)
const demoTab = ref('preview')

const code = computed(() => `<tank-seamless-scroll 
  :step-length="100" 
  :auto-scroll-threshold="${threshold.value}">
  <div v-for="(v, i) in items" :key="i" class="demo-item">Item {{ i + 1 }}</div>
</tank-seamless-scroll>

<!-- 
  auto-scroll-threshold: ${threshold.value}
  When items count > threshold, scrolling starts automatically.
  Current items: {{ count.length }}
-->`)

const addItem = () => {
  count.value = [...count.value, null]
}

const removeItem = () => {
  if (count.value.length > 1) {
    count.value = count.value.slice(0, -1)
  }
}

const resetItems = () => {
  count.value = new Array(3).fill(null)
}
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

.preview-area {
  margin-top: 8px;
}

.small-tabs {
  margin-top: 6px;
}

.small-tabs .nav-link {
  color: var(--text-secondary, #8b949e);
  background: transparent;
  border-color: var(--border-color, #30363d);
}

.small-tabs .nav-link.active {
  color: #fff;
  background: #21262d;
  border-color: var(--border-color, #30363d);
}

.controls {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-top: 12px;
  padding: 8px 12px;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 6px;
}

.controls label {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
}

.controls input {
  width: 60px;
  padding: 4px 8px;
  border-radius: 4px;
  border: 1px solid var(--border-color, #30363d);
  background: #21262d;
  color: #fff;
  font-size: 13px;
}

.hint {
  font-size: 12px;
  color: var(--text-secondary, #8b949e);
}

.button-group {
  display: flex;
  gap: 8px;
  margin-top: 8px;
}

.btn {
  padding: 6px 12px;
  border-radius: 6px;
  border: 1px solid var(--border-color, #30363d);
  background: #21262d;
  color: #fff;
  font-size: 12px;
  cursor: pointer;
  transition: all 0.2s;
}

.btn:hover {
  background: #30363d;
}

.demo .demo-item {
  padding: 12px 0;
  background: linear-gradient(135deg, #8957e5 0%, #a371f7 100%);
  margin: 4px 0;
  box-sizing: border-box;
  text-align: center;
  border-radius: 6px;
  font-size: 13px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.demo .demo-item:nth-child(2n) {
  background: linear-gradient(135deg, #6e40c9 0%, #8957e5 100%);
}
</style>
