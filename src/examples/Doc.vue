<template>
  <div class="doc-container">
    <div class="section">
      <h3>Install</h3>
      <code-show :code="codeInstall" />
    </div>

    <div class="section">
      <h3>Import</h3>
      <code-show :code="codeImport" />
    </div>

    <div class="section">
      <h3>Basic Usage</h3>
      <code-show :code="codeUsage" />
    </div>

    <div class="section">
      <h3>API Reference</h3>
      <div class="table-responsive">
        <table class="table table-dark table-striped table-bordered api-table">
          <thead>
            <tr>
              <th>Prop</th>
              <th>Type</th>
              <th>Default</th>
              <th>Description</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(row, i) in apiRows" :key="i">
              <td><code>{{ row.prop }}</code></td>
              <td>{{ row.type }}</td>
              <td><code>{{ row.default }}</code></td>
              <td>{{ row.desc }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div class="section">
      <h3>Virtual Scrolling Mode <span class="badge bg-success">New</span></h3>
      <p class="text-secondary">For large datasets (1000+ items), use virtual scrolling:</p>
      <code-show :code="codeVirtual" />
    </div>
  </div>
</template>

<script setup>
import CodeShow from "../components/codeShow.vue"

const codeInstall = `npm install --save tank-vue3-seamless-scroll`

const codeImport = `import TankSeamlessScroll from "tank-vue3-seamless-scroll"`

const codeUsage = `<div style="height:300px;">
    <tank-seamless-scroll :step-length="25" :debug="true" :reverse="false">
        <div class="demo">
            <div v-for="i in 2" :key="i">测试{{ i }}</div>
        </div>
    </tank-seamless-scroll>
</div>`

const codeVirtual = `<tank-seamless-scroll 
  :virtual="true"
  :data="bigDataList"
  :item-height="40"
  :step-length="100">
  <template #item="{ item, index }">
    <div class="row">{{ item.text }}</div>
  </template>
</tank-seamless-scroll>`

const apiRows = [
  { prop: 'step-length', type: 'Number', default: '60', desc: 'Scroll speed in pixels per second' },
  { prop: 'reverse', type: 'Boolean', default: 'false', desc: 'Reverse scrolling direction' },
  { prop: 'debug', type: 'Boolean', default: 'false', desc: 'Show debug overlay' },
  { prop: 'pauseOnHover', type: 'Boolean', default: 'true', desc: 'Pause animation on mouse hover' },
  { prop: 'virtual', type: 'Boolean', default: 'false', desc: '🆕 Enable virtual scrolling mode' },
  { prop: 'data', type: 'Array', default: '[]', desc: '🆕 Data array (virtual mode only)' },
  { prop: 'item-height', type: 'Number', default: '50', desc: '🆕 Item height in pixels (virtual mode only)' },
  { prop: 'buffer', type: 'Number', default: '5', desc: '🆕 Buffer items count (virtual mode only)' },
]
</script>

<style scoped>
.doc-container {
  color: #e6edf3;
  padding: 8px;
}
.section {
  margin-bottom: 32px;
}
h3 {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 16px;
  color: #fff;
  border-bottom: 1px solid #30363d;
  padding-bottom: 8px;
}
.api-table {
  font-size: 14px;
}
.api-table th {
  color: #8b949e;
  font-weight: 600;
}
.api-table td {
  vertical-align: middle;
}
.api-table code {
  color: #ff7b72;
  background: rgba(110, 118, 129, 0.4);
  padding: 0.2em 0.4em;
  border-radius: 6px;
  font-family: ui-monospace, SFMono-Regular, SF Mono, Menlo, Consolas, Liberation Mono, monospace;
}
.text-secondary {
  color: #8b949e !important;
}
</style>
