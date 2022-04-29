<template>
  <div id="app">
    <BestTransport/>
    <TesteComponent />
  </div>
</template>

<script>
import BestTransport from './components/BestTransport.vue'
import TesteComponent from './components/TransportForm/index.vue'

export default {
  name: 'App',
  components: {
    BestTransport,
    TesteComponent
  }
}
</script>

<style>
</style>
