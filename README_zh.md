[English](README.md) | 中文

# tank-vue3-seamless-scroll


## features

* 这是一个高性能的无缝滚动插件
* 事件监听不会失效


# 预览

![](demo.gif)

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

### api

#### step-length

* 描述：每秒移动像素高度
* 默认值: 60 (px/秒)
* 类型: Number

#### reverse

* 描述：动画将反向滚动播放
* 默认值: false
* 类型: Boolean

#### debug

* 描述：是否显示调试信息
* 默认值: false
* 类型: Boolean



