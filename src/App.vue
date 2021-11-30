<template>
  <Icon />
  <!-- <Tabs v-model:selected="x">
    <Tab title="标题1">内容一</Tab>
    <Tab title="标题2444444">内容二</Tab>
  </Tabs> -->
</template>

<script>
import Icon from './components/icon.vue'
// import Tabs from './components/Tab/tabs.vue'
// import Tab from './components/Tab/tab.vue'
// import { ref } from 'vue'
// import Icon from './components/icon.vue'
export default {
  name: 'App',
  components: {
    // Tabs,
    // Tab,
    Icon
  },
  // setup() {
  //   const x = ref('标题1')
  //   return {
  //     x
  //   }
  // }
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
