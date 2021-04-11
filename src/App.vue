<template>
  <div id="app">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>
<style lang="scss">
@import 'scss/variables.scss';
html, body, #app {
  width: 100%;
  height: 100%;
}
#app {
  font-family: Quicksand;
  // font-size: 75px;
  font-style: normal;
  font-weight: 400;
  // line-height: 94px;
  letter-spacing: 0em;
  text-align: left;
  background: $pink;
  display: flex;
}
</style>
