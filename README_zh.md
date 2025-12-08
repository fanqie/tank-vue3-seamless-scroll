[English](README.md) | 中文

# tank-vue3-seamless-scroll

> **2.0 版本** - 现在使用 CSS Keyframes 驱动！🚀

## V2.0 新特性

- ✅ **CSS 原生动画**：丝滑流畅的滚动体验
- ✅ **虚拟滚动**：轻松处理 10万+ 数据
- ✅ **GPU 加速**：硬件加速变换
- ✅ **零依赖**：无需外部库
- ✅ **100% API 兼容**：无缝替换 v1.x
- ✅ **智能渲染**：使用 `content-visibility` 仅渲染可见内容

## 功能特性

* 🎯 高性能无缝滚动
* 🔄 无限循环动画
* ⏸️ 悬停暂停
* 🔃 反向滚动支持
* 📊 海量数据虚拟滚动
* 🐛 内置调试模式

## 运行环境

* Vue 3.x
* Node.js 14.15.0+
* 支持 CSS Keyframes 的现代浏览器

## 快速开始

### 安装

```shell
npm install --save tank-vue3-seamless-scroll
```

### 导入

```js
import TankSeamlessScroll from "tank-vue3-seamless-scroll"
```

### 使用组件

```html

<div style="height:300px;">
    <tank-seamless-scroll :step-length="25" :debug="true" :reverse="false">
        <div class="demo">
            <div v-for="i in 2" :key="i">测试{{ i }}</div>
        </div>
    </tank-seamless-scroll>
</div>
```

### API 参考

| 属性 | 类型 | 默认值 | 说明 |
|------|------|---------|-------------|
| `step-length` | Number | 60 | 滚动速度，每秒像素 |
| `reverse` | Boolean | false | 反向滚动 |
| `debug` | Boolean | false | 显示调试信息 |
| `pauseOnHover` | Boolean | true | 鼠标悬停时暂停 |
| `virtual` | Boolean | false | 🆕 启用虚拟滚动模式 |
| `data` | Array | [] | 🆕 数据数组（仅虚拟模式） |
| `item-height` | Number | 50 | 🆕 每项高度（仅虚拟模式） |
| `buffer` | Number | 5 | 🆕 缓冲区项目数（仅虚拟模式） |

### 虚拟滚动模式 🆕

对于大数据量（1000+ 条），使用虚拟滚动：

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

## 许可证

MIT
