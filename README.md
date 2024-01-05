English | [中文](./README_zh.md)

# tank-vue3-seamless-scroll

## features

* This is a high-performance seamless scrolling plug-in
* Event monitoring will not fail

## Warning

If you plan to use this plugin for a large amount of data, Please choose carefully,we not done any virtual optimization
for targeted memory and CPU of Vue third-party components.

## engine
* vue3
* nodejs 14.15.0+

## preview

Click on the image to play the animation
[![demo](./demo.jpg)](https://user-images.githubusercontent.com/466966/233253193-66d316da-3803-41dc-b115-3d74ec2b8d8d.mp4)
[demo online](https://fanqie.github.io/tank-vue3-seamless-scroll/dist/index.html)

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

### api

#### step-length

* desc：Pixel height moved per second
* default: 60 (px/second)
* type: Number

#### reverse

* desc：The animation will play in reverse scrolling
* default: false
* type: Boolean

#### debug

* desc：Whether to display debugging information
* default: false
* type: Boolean

#### pauseOnHover:

* desc：Whether to pause when the mouse hovers
* default: true
* type: Boolean
