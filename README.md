English | [中文](./README_zh.md)

# tank-vue3-seamless-scroll

> **Version 2.0** - Now powered by CSS Keyframes! 🚀

## What's New in V2.0

- ✅ **CSS-based Animation**: Native CSS keyframes for buttery smooth scrolling
- ✅ **Virtual Scrolling**: Handle 100,000+ items with ease
- ✅ **GPU Accelerated**: Hardware-accelerated transforms
- ✅ **Zero Dependencies**: No external libraries required
- ✅ **100% API Compatible**: Drop-in replacement for v1.x
- ✅ **Smart Rendering**: Only renders visible content with `content-visibility`

## Features

* 🎯 High-performance seamless scrolling
* 🔄 Infinite loop animation
* ⏸️ Pause on hover
* 🔃 Reverse direction support
* 📊 Virtual scrolling for massive datasets
* 🐛 Built-in debug mode

## Requirements

* Vue 3.x
* Node.js 14.15.0+
* Modern browser with CSS Keyframes support

## Quick Start

### install

```shell
npm install --save tank-vue3-seamless-scroll
```

### import

```js
import TankSeamlessScroll from "tank-vue3-seamless-scroll"
```

### use component

```html

<div style="height:300px;">
    <tank-seamless-scroll :step-length="25" :debug="true" :reverse="false">
        <div class="demo">
            <div v-for="i in 2" :key="i">测试{{ i }}</div>
        </div>
    </tank-seamless-scroll>
</div>
```

### API Reference

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `step-length` | Number | 60 | Scroll speed in pixels per second |
| `reverse` | Boolean | false | Reverse scrolling direction |
| `debug` | Boolean | false | Show debug overlay |
| `pauseOnHover` | Boolean | true | Pause animation on mouse hover |
| `virtual` | Boolean | false | 🆕 Enable virtual scrolling mode |
| `data` | Array | [] | 🆕 Data array (virtual mode only) |
| `item-height` | Number | 50 | 🆕 Item height in pixels (virtual mode only) |
| `buffer` | Number | 5 | 🆕 Buffer items count (virtual mode only) |

### Virtual Scrolling Mode 🆕

For large datasets (1000+ items), use virtual scrolling:

```html
<tank-seamless-scroll 
  :virtual="true"
  :data="bigDataList"
  :item-height="40"
  :step-length="100">
  <template #item="{ item, index }">
    <div class="row">{{ item.text }}</div>
  </template>
</tank-seamless-scroll>
```

## License

MIT
