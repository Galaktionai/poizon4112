<template>
  <div id="app">
    <v-header />
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div> 
</template>

<script>
import vHeader from './components/v-header'

export default {
  name: 'App',
  components: {
    vHeader
  }
}
</script>

<style>
  
</style>