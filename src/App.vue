<template>
  <div id="main">
    <div class="noprint">
      <h1>Generátor příkladů malé násobilky</h1>
      <form @submit.prevent="generateExercises">
        <label for="maxNumber">Největší činitel 1: </label>
        <input
          type="number"
          id="maxFactor1"
          v-model.number="maxFactor1"
          min="1"
          max="10"
          required
        />
        <input type="range" v-model.number="maxFactor1" min="1" max="10" step="1">
        <br /><br />
        <label for="maxNumber">Největší činitel 2: </label>
        <input
          type="number"
          id="maxFactor2"
          v-model.number="maxFactor2"
          min="1"
          max="10"
          required
        />
        <input type="range" v-model.number="maxFactor2" min="1" max="10" step="1">
        <br /><br />
        <label for="maxNumber">Vynechat nuly: </label>
        <input
          type="checkbox"
          id="excludeZeroes"
          v-model.number="excludeZeroes"
        />
        <br /><br />
        <label for="maxNumber">Neopakovat příklady: </label>
        <input
          type="checkbox"
          id="exludeDuplicates"
          v-model.number="excludeDuplicates"
        />
        <br /><br />
        <label for="numExercises">Max. počet příkladů: </label>
        <input
          type="number"
          id="numExercises"
          v-model.number="numExercises"
          min="1"
          max="100"
          required
        />
        <input type="range" v-model.number="numExercises" min="1" max="100" step="1">
        <br /><br />
        <label for="numColumns">Počet sloupečků: </label>
        <input
          type="number"
          id="numColumns"
          v-model.number="numColumns"
          min="1"
          max="5"
          required
        />
        <input type="range" v-model.number="numColumns" min="1" max="10" step="1">
        <br /><br />
        <label for="numRows">Max. počet řádků: </label>
        <input
          type="number"
          id="numRows"
          v-model.number="numRows"
          min="1"
          max="50"
          required
        />
        <input type="range" v-model.number="numRows" min="1" max="100" step="1">
        <br /><br />
        <button type="submit">Generuj</button>&nbsp;
        <button @click="printWindow()">Tisk</button>
      </form>
      <br /><br />
    </div>
    <div id="printarea" v-if="exercises.length > 0">
      <table>
        <tbody>
          <tr v-for="(exerciseRow, rowIndex) in exerciseRows" :key="rowIndex">
            <td
              v-for="(exercise, exerciseIndex) in exerciseRow"
              :key="exerciseIndex"
            >
              <div>
                <span style="display: inline-block; width: 3em">{{
                  exercise
                }}</span
                >=&nbsp; &nbsp;_____<span
                  style="display: inline-block; width: 2em"
                ></span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
      <br />
    </div>
    <div class="small">
      <a href="https://nasobilka.fletna.online">nasobilka.fletna.online</a>: s
      pár <a href="https://github.com/radkoa">úpravami</a> naprogramovalo
      <a href="https://ai.com">ChatGPT</a>.
    </div>
  </div>
</template>

<script>
export default {
  name: "Generator",
  created() {
    document.title = "Generátor příkladů malé násobilky";
  },
  data() {
    return {
      numExercises: 76,
      maxFactor1: 9,
      maxFactor2: 9,
      excludeZeroes: true,
      excludeDuplicates: false,
      numColumns: 4,
      numRows: 19,
      exercises: [],
    };
  },
  computed: {
    exerciseRows() {
      const exerciseRows = [];
      for (let i = 0; i < this.numRows; i++) {
        const start = i * this.numColumns;
        const end = start + this.numColumns;
        exerciseRows.push(this.exercises.slice(start, end));
      }
      return exerciseRows;
    },
    maxNumExercises() {
      return (this.maxFactor1 + 1) * (this.maxFactor2 + 1);
    },
    numExercisesAdjusted() {
      if (this.excludeDuplicates) {
        return Math.min(this.numExercises, this.maxNumExercises);
      } else {
        return this.numExercises;
      }
    },
  },
  methods: {
    generateExercises() {
      // Clear the exercises array
      this.exercises = [];

      // Create an array to store all possible factor pairs
      const factorPairs = [];
      for (let i = 0; i <= this.maxFactor1; i++) {
        for (let j = 0; j <= this.maxFactor2; j++) {
          factorPairs.push([i, j]);
        }
      }

      // Filter out any factor pairs that include a zero if excludeZeroes is set to true
      if (this.excludeZeroes) {
        factorPairs.splice(0, factorPairs.length, ...factorPairs.filter(([num1, num2]) => num1 !== 0 && num2 !== 0));
      }

      // Reduce the numExercises if excludeDuplicates is true
      if (this.excludeDuplicates) {
        this.numExercises = Math.min(factorPairs.length, this.numExercises);
      }

      // Generate exercises by randomly selecting factor pairs from the remaining pairs
      while (this.exercises.length < this.numExercises) {
        const index = Math.floor(Math.random() * factorPairs.length);
        const [num1, num2] = factorPairs[index];
        if (this.excludeDuplicates) {
          factorPairs.splice(index, 1);
        }

        const exercise = `${num1} · ${num2}`;

        this.exercises.push(exercise);
      }
    },
    printWindow() {
      window.print();
    },
  },
};
</script>

<style scoped>
#main {
  font-family: "Open Sans", sans-serif;
}
table {
  line-height: 3em;
}
.small {
  font-size: 7pt;
  color: silver;
  text-align: right;
}
.small a {
  color: silver;
  text-decoration: none;
}
@media print {
  .noprint {
    display: none;
  }
}
</style>
