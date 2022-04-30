<template>
  <div id="app">
    <NavBar/>
    <MainSection />
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue'
import MainSection from './components/MainSection.vue'

export default {
  name: 'App',
  components: {
    NavBar,
    MainSection
  }
}
</script>

<style>
</style>
