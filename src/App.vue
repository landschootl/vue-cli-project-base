<template>
  <div id="app">
    <ManageProducts/>
  </div>
</template>

<script>
import ManageProducts from './components/ManageProducts.vue'

export default {
  name: 'App',
  components: {
    ManageProducts
  }
}
</script>

<style>
#app {
  margin: 60px;
}
</style>
