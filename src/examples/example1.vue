<template>
  <div style="height:300px;">
    <tank-seamless-scroll :step-length="stepLength" :debug="debug" :reverse="reverse">
      <div class="demo">
        <div v-for="i in 2" :key="i">测试{{ i }}</div>
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
  background-color: #800000;
  margin: 5px 10px 0px 0px;
  text-align: center;
  border-radius: 5px;
}

.demo div:nth-child(2n) {
  background-color: #ff4673;
}
</style>
