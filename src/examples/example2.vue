<template>
  <div style="height:500px">
    <tank-seamless-scroll  :step-length="stepLength" :debug="debug" :reverse="reverse">
      <div class="demo">
        <div v-for="i in 6" :key="i">测试{{i}}</div>
      </div>
    </tank-seamless-scroll>
  </div>

</template>
<script setup>
import {ref} from 'vue'
import TankSeamlessScroll from "../Index.vue"

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
  }
})
const count = ref(0)
</script>
<style scoped>
.demo {
  color: #fff;
}

.demo div {
  padding: 10px 20px;
  background-color: #05ad01;
  margin: 5px 10px 0px 0px;
  text-align: center;
  border-radius: 5px;
  box-shadow:  0px 5px 20px  rgba(0,0,0,.1);
}

.demo div:nth-child(2n) {
  background-color: #025600;
}
</style>
