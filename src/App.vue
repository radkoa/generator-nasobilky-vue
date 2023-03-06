<template>
  <div id="main">
    <div class="noprint">
      <h1>Generátor příkladů malé násobilky</h1>
      <form @submit.prevent="generateExercises">
        <label for="numExercises">Počet příkladů: </label>
        <input
          type="number"
          id="numExercises"
          v-model.number="numExercises"
          min="1"
          max="100"
          required
        />
        <br /><br />
        <label for="maxNumber">Největší činitel 1: </label>
        <input
          type="number"
          id="maxFactor1"
          v-model.number="maxFactor1"
          min="1"
          max="10"
          required
        />
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
        <br /><br />
        <label for="numRows">Počet řádků: </label>
        <input
          type="number"
          id="numRows"
          v-model.number="numRows"
          min="1"
          max="50"
          required
        />
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
  data() {
    return {
      numExercises: 30,
      maxFactor1: 2,
      maxFactor2: 10,
      numColumns: 3,
      numRows: 10,
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
  },
  methods: {
    generateExercises() {
      this.exercises = [];
      this.answers = [];
      for (let i = 0; i < this.numExercises; i++) {
        const num1 = Math.floor(Math.random() * (this.maxFactor1 + 1));
        const num2 = Math.floor(Math.random() * (this.maxFactor2 + 1));
        const exercise = `${num1} · ${num2}`;
        this.exercises.push(exercise);
      }
    },
    printWindow() {		
	    window.print();
    }
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