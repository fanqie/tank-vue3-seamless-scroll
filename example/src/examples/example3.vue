<template>
  <div class="demo" :style="{height:`${height}px`}">
    <h2 v-if="title">{{title}}</h2>
    <tank-seamless-scroll :step-length="stepLength" :debug="debug" :reverse="reverse">
      <div v-for="(v,i) in count" :key="i">value:{{ v }},key:{{ i }}</div>
    </tank-seamless-scroll>
  </div>

</template>
<script setup>
import {ref, onMounted, onUnmounted} from 'vue'
import TankSeamlessScroll from "../../../index.js";

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
  },
})
const count = ref(new Array(200).fill(null))

</script>
<style scoped>
h2{
  color: #01092c;
  text-align: center;
}
.demo {
  color: #fff;
  text-shadow: black;
  height: 100px;
}

.demo div {
  padding: 10px 0px;
  background-color: #ffea5e;
  margin: 5px;
  box-sizing: border-box;
  text-align: center;
  border-radius: 5px;
}

.demo div:nth-child(2n) {
  background-color: #ff6625;
}
</style>
