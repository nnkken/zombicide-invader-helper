<template>
  <div>
    <div>
      Dices:
      <div class="select-dice">
        <div v-for="i in 10" v-bind:key="i" class="select-dice-button-container">
          <button @click="dice = i">{{ i }}</button>
        </div>
      </div>
      <div class="select-dice">
        <input v-model="dice" type="range" name="dice" min="1" max="10">
      </div>
    </div>
    <div>
      Criteria:
      <div class="select-criteria">
        <div v-for="i in 6" v-bind:key="i" class="select-criteria-button-container">
          <button @click="criteria = i">{{ i }}+</button>
        </div>
      </div>
      <div class="select-criteria">
        <input v-model="criteria" type="range" name="criteria" min="1" max="6">
      </div>
    </div>
    <div>
      <h4>Hit prob table for {{ dice }} dice {{ criteria }}+:</h4>
      <table>
        <tr>
          <th>Hits</th>
          <th>Pass</th>
          <th>Exact</th>
        </tr>
        <tr v-for="(output, hit) in table" v-bind:key="`${output}${hit}`">
          <td>&#x2265; {{ hit }}</td>
          <td>{{ output.revAcc | percent }}</td>
          <td>{{ output.prob | percent }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
function nCr(n, r) {
    let ans = 1;
    for (let i = 0; i < r; i += 1) {
      ans *= n - i;
      ans /= i + 1;
    }
    return ans;
}

function calProb(dice, criteria, hit) {
    let pMiss = (criteria - 1) / 6;
    let pHit = 1 - pMiss;
    return nCr(dice, hit) * Math.pow(pHit, hit) * Math.pow(pMiss, dice - hit);
}

export default {
  name: 'ProbCalculator',
  data() {
    return {
      dice: 5,
      criteria: 5,
    };
  },
  filters: {
    percent(p) {
      return `${(p * 100).toFixed(2)}%`;
    },
  },
  computed: {
    table() {
      const output = {};
      let revAcc = 1;
      for (let hit = 0; hit <= this.dice; hit += 1) {
        const prob = calProb(this.dice, this.criteria, hit);
        output[hit] = { prob, revAcc };
        revAcc -= prob;
      }
      return output;
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
input[type=range] {
  width: 100%;
}

.select-dice {
  width: 100%;
  max-width: 25rem;
  display: flex;
}
.select-dice .select-dice-button-container {
  width: 100%;
}
.select-dice button {
  width: 100%;
}

.select-criteria {
  width: 100%;
  max-width: 25rem;
  display: flex;
}
.select-criteria .select-criteria-button-container {
  width: 100%;
}
.select-criteria button {
  width: 100%;
}
</style>
