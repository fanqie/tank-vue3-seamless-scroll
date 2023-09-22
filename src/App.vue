<script setup>
import Example1 from './examples/example1.vue'
import Example2 from './examples/example2.vue'
import Example3 from './examples/example3.vue'
import ExampleVirtual from './examples/example-virtual.vue'
</script>

<template>
  <div>
    <h2>【hint】Please enter the example component to view the code<br/>
      <small>By：tank-vue3-seamless-scroll</small>
    </h2>
    <hr/>
    <div class="examples-box">

      <example2 :height="100" title="↓↓↓ normal ↓↓↓"/>
      <example2 :step-length="32" :reverse="true" :height="380" title="↓↓↓ reverse  ↓↓↓"/>
      <example2 :step-length="15" :debug="true" :height="550" title="↓↓↓ debug  ↓↓↓"/>
      <example3 :step-length="200" :debug="false" :height="260" title="↓↓↓ many row  ↓↓↓"/>
      <example1 :step-length="120" :debug="false" :height="450" title="↓↓↓ append  ↓↓↓"/>
      <!--    currently not supported -->
      <!--      <ExampleVirtual :step-length="120" :debug="false" :height="450" title="↓↓↓ virtual ↓↓↓"/>-->
    </div>
  </div>
</template>

<style scoped>
.examples-box {
  display: flex;
  width: 100%;
}

.examples-box > * {
  width: 220px;
  flex: 1;
}
</style>
