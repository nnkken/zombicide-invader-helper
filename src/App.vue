<template>
  <div id="prob-calculator">
    <h2>Probability Calculator</h2>
    <ProbCalculator />
    <div class="likecoin-embed likecoin-like-button">
      <div>
        <iframe scrolling="no" frameborder="0" :src="src" />
      </div>
    </div>
  </div>
</template>

<script>
import ProbCalculator from './components/ProbCalculator.vue'

export default {
  name: 'app',
  components: {
    ProbCalculator
  },
  computed: {
    src() {
      return `https://button.like.co/in/embed/chungwu/button?referrer=${encodeURIComponent('https://zombicide-invader-helper.netlify.com')}`;
    },
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

.likecoin-like-button {
  max-width: 485px;
  max-height: 240px;
  width: 100%;
}
.likecoin-like-button > div {
  position: relative;
  padding-top: 49.48454%;
}
.likecoin-like-button > div iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
