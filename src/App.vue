<script setup>
import { ref } from 'vue'
import Example1 from './examples/example1.vue'
import Example2 from './examples/example2.vue'
import Example3 from './examples/example3.vue'
import ExampleVirtual from './examples/example-virtual.vue'
import ExampleComplex from './examples/example-complex.vue'
import ExampleAutoScroll from './examples/example-auto-scroll.vue'
import Doc from './examples/Doc.vue'

// 一级导航
const mainModes = [
  { key: 'doc', label: 'Documentation' },
  { key: 'demo', label: 'Live Examples' }
]
const currentMode = ref('doc')

// 示例导航
const exampleTabs = [
  { key: 'normal', label: 'Normal', component: Example2, props: { height:160, title:'Normal' } },
  { key: 'reverse', label: 'Reverse', component: Example2, props: { stepLength:32, reverse:true, height:160, title:'Reverse' } },
  { key: 'debug', label: 'Debug Mode', component: Example2, props: { stepLength:15, debug:true, height:200, title:'Debug Mode' } },
  { key: 'multi', label: 'Multi Row', component: Example3, props: { stepLength:200, debug:false, height:200, title:'Multi Row' } },
  { key: 'dynamic', label: 'Dynamic Append', component: Example1, props: { stepLength:120, debug:false, height:200, title:'Dynamic Append' } },
  { key: 'autoScroll', label: 'Auto Threshold', component: ExampleAutoScroll, props: { height:280, title:'Auto Scroll Threshold' } },
  { key: 'virtual', label: 'Virtual (100K)', component: ExampleVirtual, props: { stepLength:1000, debug:true, height:200, count:100000, title:'Virtual (100K)' } },
  { key: 'complex', label: 'Complex (5K)', component: ExampleComplex, props: { stepLength:120, debug:true, reverse:true, height:200, count:5000, title:'Complex (5K)' } }
]
const activeExample = ref(exampleTabs[0].key)
</script>

<template>
  <div class="app-container">
    <header class="app-header">
      <div class="header-top">
        <a href="https://github.com/fanqie/tank-vue3-seamless-scroll/" class="logo-link">
          <svg height="28" viewBox="0 0 16 16" width="28" fill="currentColor">
            <path d="M8 0c4.42 0 8 3.58 8 8a8.013 8.013 0 0 1-5.45 7.59c-.4.08-.55-.17-.55-.38 0-.27.01-1.13.01-2.2 0-.75-.25-1.23-.54-1.48 1.78-.2 3.65-.88 3.65-3.95 0-.88-.31-1.59-.82-2.15.08-.2.36-1.02-.08-2.12 0 0-.67-.22-2.2.82-.64-.18-1.32-.27-2-.27-.68 0-1.36.09-2 .27-1.53-1.03-2.2-.82-2.2-.82-.44 1.1-.16 1.92-.08 2.12-.51.56-.82 1.28-.82 2.15 0 3.06 1.86 3.75 3.64 3.95-.23.2-.44.55-.51 1.07-.46.21-1.61.55-2.33-.66-.15-.24-.6-.83-1.23-.82-.67.01-.27.38.01.53.34.19.73.9.82 1.13.16.45.68 1.31 2.69.94 0 .67.01 1.3.01 1.49 0 .21-.15.45-.55.38A7.995 7.995 0 0 1 0 8c0-4.42 3.58-8 8-8Z"></path>
          </svg>
          <span class="logo-text">tank-vue3-seamless-scroll</span>
          <span class="version-badge">v2.0</span>
        </a>

        <!-- Main Navigation Pills -->
        <ul class="nav nav-pills main-nav">
          <li class="nav-item" v-for="m in mainModes" :key="m.key">
            <a href="#" class="nav-link" :class="{ active: currentMode===m.key }" @click.prevent="currentMode=m.key">
              {{ m.label }}
            </a>
          </li>
        </ul>
      </div>

      <p class="app-desc">
        View <a href="https://github.com/fanqie/tank-vue3-seamless-scroll/tree/main/src/examples" target="_blank">example code</a>
        <span class="highlight">Virtual scrolling, easy for massive data</span>
      </p>
    </header>

    <!-- Content Area -->
    <div class="content-area">
      
      <!-- Documentation Mode -->
      <div v-if="currentMode === 'doc'" class="doc-wrapper">
        <div class="card example-card">
          <Doc />
        </div>
      </div>

      <!-- Examples Mode -->
      <div v-else class="examples-wrapper">
        <ul class="nav nav-tabs examples-tabs">
          <li class="nav-item" v-for="t in exampleTabs" :key="t.key">
            <a href="#" class="nav-link" :class="{ active: activeExample===t.key }" @click.prevent="activeExample=t.key">{{ t.label }}</a>
          </li>
        </ul>
        <div class="tab-content">
          <div v-for="t in exampleTabs" :key="t.key" class="tab-pane" :class="{ 'active show': activeExample===t.key }" v-show="activeExample===t.key">
            <div class="card example-card">
              <component :is="t.component" v-bind="t.props" />
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<style>
:root {
  --bg-primary: #0d1117;
  --bg-secondary: #161b22;
  --bg-card: #21262d;
  --border-color: #30363d;
  --text-primary: #e6edf3;
  --text-secondary: #8b949e;
  --accent-color: #58a6ff;
  --accent-green: #3fb950;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: var(--bg-primary);
  color: var(--text-primary);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
  min-height: 100vh;
}
</style>

<style scoped>
.app-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 24px;
}

.header-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
  flex-wrap: wrap;
  gap: 16px;
}

.logo-link {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
  color: var(--text-primary);
  transition: opacity 0.2s;
}

.logo-link:hover {
  opacity: 0.8;
}

.logo-text {
  font-size: 22px;
  font-weight: 600;
  letter-spacing: -0.5px;
}

.version-badge {
  background: linear-gradient(135deg, var(--accent-color), var(--accent-green));
  color: #fff;
  font-size: 11px;
  font-weight: 600;
  padding: 3px 8px;
  border-radius: 12px;
}

/* Main Nav (Pills) */
.main-nav .nav-link {
  color: var(--text-secondary);
  font-weight: 500;
  border-radius: 20px;
  padding: 6px 20px;
  transition: all 0.2s;
}
.main-nav .nav-link:hover {
  color: #fff;
  background: rgba(110, 118, 129, 0.1);
}
.main-nav .nav-link.active {
  background: var(--accent-color);
  color: #fff;
}

.app-desc {
  color: var(--text-secondary);
  font-size: 14px;
  margin-bottom: 24px;
}

.app-desc a {
  color: var(--accent-color);
  text-decoration: none;
}

.app-desc a:hover {
  text-decoration: underline;
}

.highlight {
  display: inline-block;
  margin-left: 12px;
  padding: 2px 10px;
  background: rgba(63, 185, 80, 0.15);
  color: var(--accent-green);
  border-radius: 4px;
  font-size: 13px;
}

/* Example Tabs */
.examples-tabs { margin-bottom: 12px; border-bottom-color: var(--border-color); }
.examples-tabs .nav-link { 
  color: var(--text-secondary); 
  background: transparent; 
  border: 1px solid transparent;
  border-bottom: none;
  margin-bottom: -1px;
}
.examples-tabs .nav-link:hover {
  color: #fff;
  border-color: transparent;
}
.examples-tabs .nav-link.active { 
  color: #fff; 
  background: #21262d; 
  border-color: var(--border-color);
  border-bottom-color: #21262d; /* Merge with card */
}

.example-card { 
  background: var(--bg-card); 
  border: 1px solid var(--border-color); 
  border-radius: 6px; 
  /* Top-left corner needs to be sharp if active tab is first, but bootstrap handles this generically. 
     We can just keep border radius for the card. */
  border-top-left-radius: 0;
  padding: 16px; 
  transition: border-color 0.2s, box-shadow 0.2s; 
  overflow:auto 
}
.doc-wrapper .example-card {
  border-top-left-radius: 6px; /* Reset for doc mode */
}
.example-card:hover { border-color: var(--accent-color); box-shadow: 0 0 20px rgba(88, 166, 255, 0.1); }
</style>
