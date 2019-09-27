<template>
  <div id="prob-calculator">
    <h2>Probability Calculator</h2>
    <ProbCalculator />
  </div>
</template>

<script>
import ProbCalculator from './components/ProbCalculator.vue'

export default {
  name: 'app',
  components: {
    ProbCalculator
  },
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
