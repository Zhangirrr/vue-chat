<template>
  <div id="app">
    <Myheader/>
    <router-view/>
    <Myfooter/>
  </div>
</template>

<script>
import Myheader from '@/components/Myheader.vue'
import Myfooter from '@/components/Myfooter.vue'

export default {
  name: 'AppView',
  components: {
    Myheader,
    Myfooter
  }
}
</script>

<style lang="less">

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body{
  margin: 0;
  padding: 0;
}
nav {
  // padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
