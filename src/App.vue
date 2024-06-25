<template>
  <div id="app">
    <ProductFunnel :qrLink="'https://manager.aiscreen.io/login'" />
  </div>
</template>

<script>
import ProductFunnel from '@/components/ProductFunnel.vue'
export default {
  name: 'App',
  components: {
    ProductFunnel,
  },
}
</script>

<style lang="scss">
@import './scss/main.scss';
</style>
