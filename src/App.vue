<template>
  <MenuVue />
</template>

<script>
import Menu from './components/WalkingMenu.vue'

export default {
  components: {
    MenuVue: Menu,
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
