<template>
  <div>
    <div>
      Dices:
      <div class="select-container">
        <div v-for="i in 10" v-bind:key="i" class="select-button-container dice">
          <button @click="dice = i">{{ i }}</button>
        </div>
        <div class="select-range-container dice">
          <input v-model="dice" type="range" name="dice" min="1" max="10">
        </div>
      </div>
    </div>
    <div>
      Criteria:
      <div class="select-container">
        <div v-for="c in criteriaList" v-bind:key="c" class="select-button-container criteria">
          <button @click="criteria = c">{{ c }}+</button>
        </div>
        <div class="select-range-container criteria">
          <input v-model="criteria" type="range" name="criteria" min="2" max="6">
        </div>
      </div>
    </div>
    <div>
      <h4>Hit prob table for {{ dice }} dice {{ criteria }}+:</h4>
      <table class="table-hit-prob">
        <tr>
          <th>Hits</th>
          <th>Pass</th>
          <th>Exact</th>
        </tr>
        <tr v-for="(output, hit) in table" v-bind:key="`${output}${hit}`">
          <td>&#x2265; {{ hit }}</td>
          <td :style="getColorByProb(output.revAcc)">{{ output.revAcc | percent }}</td>
          <td :style="getColorByProb(output.prob)">{{ output.prob | percent }}</td>
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
  methods: {
    getColorByProb(p) {
      const val = p * 100;
      const state = Math.floor(val / 50);
      let r = 255;
      let g = 255;
      switch (state) {
        case 0: { // from red to yellow
          g = Math.round(p * 2 * 255);
          break;
        }
        case 1: { // from yellow to green
          r = 255 -  Math.round((p - 0.5) * 2 * 255);
          break;
        }
        case 2: { // p == 1
          r = 0;
          break;
        }
        default: break;
      }
      return {
        color: `rgb(${r}, ${g}, 0)`,
        'background-color': '#696969',
      };
    }
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
    criteriaList() {
      return [2, 3, 4, 5, 6];
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

.select-container {
  width: 100%;
  max-width: 25rem;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.select-container button {
  width: 100%;
}
.select-button-container {
  width: 100%;
}
.select-button-container.dice {
  flex-basis: 10%;
}
.select-button-container.criteria {
  flex-basis: 20%;
}
.select-range-container {
  width: 100%;
}
.select-range-container.dice {
  flex-basis: 90%;
}
.select-range-container.criteria {
  flex-basis: 80%;
}
.select-criteria .select-criteria-button-container {
  width: 100%;
}
.select-criteria button {
  width: 100%;
}

.table-hit-prob {
  border-collapse: collapse;
}

.table-hit-prob td, .table-hit-prob th{
  border: 1px solid #ddd;
  padding: 8px;
}

</style>
