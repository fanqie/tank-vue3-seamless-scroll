<script setup>
import Example1 from './examples/example1.vue'
import Example2 from './examples/example2.vue'
import Example3 from './examples/example3.vue'
</script>

<template>
  <div class="examples-box">
    <example1  :debug="true" :reverse="false"/>
    <example2 :step-length="32" :debug="true" :reverse="true"/>
    <example1 :step-length="15" :debug="true" :reverse="false"/>
    <example3 :step-length="200" :debug="false" :reverse="true"/>
    <example1 :step-length="120" :debug="false" :reverse="false"/>
  </div>
</template>

<style scoped>
.examples-box {
  display: flex;
  width:100%;
}

.examples-box > * {
  width: 120px;
  flex: 1;
}
</style>
