<template>
  <div id="app">
    <modal></modal>
    <tab-master></tab-master>
  </div>
</template>

<script>
import Modal from './components/Modal.vue';
import TabMaster from './components/TabMaster.vue';

export default {
  name: 'app',
  components: {
    Modal, TabMaster
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
