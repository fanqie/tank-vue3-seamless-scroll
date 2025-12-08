<template>
  <div class="code-block">
    <div class="code-toolbar">
      <button class="btn btn-sm btn-secondary" @click="copyCode">{{ copied ? 'Copied' : 'Copy' }}</button>
    </div>
    <pre class="code-show"><code>
      <div v-for="(line, idx) in lines" :key="idx" class="code-line">
        <span class="line-number">{{ idx + 1 }}</span>
        <span class="line-text">{{ line }}</span>
      </div>
    </code></pre>
  </div>
  </template>
<script setup>
import { computed, ref } from 'vue'
const props = defineProps({
  code: { type: String, default: '' }
})
const lines = computed(() => (props.code || '').replace(/\t/g, '  ').split(/\r?\n/))
const copied = ref(false)
const copyCode = async () => {
  try {
    await navigator.clipboard.writeText(props.code)
    copied.value = true
    setTimeout(() => (copied.value = false), 1500)
  } catch (e) {
    copied.value = false
  }
}
</script>
<style scoped>
.code-block { margin-top: 10px; border: 1px solid var(--border-color, #30363d); border-radius: 6px; overflow: hidden; background-color: #21262d; }
.code-toolbar { display: flex; justify-content: flex-end; padding: 8px; background: #161b22; border-bottom: 1px solid var(--border-color, #30363d); }
.code-show { background-color: #0d1117; margin: 0; padding: 12px; overflow: auto; white-space: pre; font-family: Consolas, Monaco, "Andale Mono", "Ubuntu Mono", monospace; font-size: 13px; line-height: 1.5; }
.code-line { display: flex; }
.line-number { width: 40px; flex-shrink: 0; text-align: right; padding-right: 10px; color: #8b949e; user-select: none; }
.line-text { white-space: pre-wrap; color: #e6edf3; }
</style>
