<script setup>
import HelloWorld from './components/progress.vue'
import Slider from './components/slider.vue'
</script>

<template>
  <div class="content">
    <Slider />
    <HelloWorld count="0" />
    <HelloWorld count="50" />
    <HelloWorld count="70" />
  </div>
</template>

<style lang="less" scoped>
.content {
  display: flex;
  justify-content: space-evenly;
}
</style>
