<template>
  <div class="demo" :style="{height:`${height}px`}">
    <h2 v-if="title">{{title}}</h2>
    <tank-seamless-scroll :step-length="stepLength" :debug="debug" :reverse="reverse">
      <div v-for="i in 6" :key="i">test{{ i }}</div>
    </tank-seamless-scroll>
  </div>

</template>
<script setup>
import {ref} from 'vue'
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
  }

})
const count =  ref(new Array(5).fill(null))
</script>
<style scoped>
h2{
  color: #01092c;
  text-align: center;
}
.demo {
  color: #fff;
}

.demo div {
  padding: 10px 0px;
  background-color: #05ad01;
  margin: 5px;
  box-sizing: border-box;
  text-align: center;
  border-radius: 5px;

  box-shadow: 0px 5px 20px rgba(0, 0, 0, .1);
}

.demo div:nth-child(2n) {
  background-color: #025600;
}
</style>
