<template>
  <Clock></Clock>
</template>

<script>
import Clock from './components/Clock.vue';
// import Alarm from './components/Alarm.vue';

export default {
    name: "App",
    components: {Clock}
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  text-align: center;
  color: #2c3e50;
  position: absolute;
  top: 45%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
