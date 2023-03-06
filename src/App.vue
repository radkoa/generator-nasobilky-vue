<template>
  <div id="main">
    <h1>Generátor příkladů násobilky</h1>
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
      <button type="submit">Generuj</button>
    </form>
    <br /><br />
    <div id="printable" v-if="exercises.length > 0">
      <table>
        <tbody>
          <tr v-for="(exerciseRow, rowIndex) in exerciseRows" :key="rowIndex">
            <td
              class="fitwidth"
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
      S pár <a href="https://github.com/radkoa">úpravami</a> naprogramovalo
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
      answers: [],
      showResults: false,
      correctAnswers: 0,
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
        this.answers.push("");
      }
      this.showResults = false;
      this.correctAnswers = 0;
    },
    checkAnswers() {
      let correct = 0;
      for (let i = 0; i < this.numExercises; i++) {
        const [num1, num2] = this.exercises[i].split(" x ");
        const expectedAnswer = num1 * num2;
        if (this.answers[i] == expectedAnswer) {
          correct++;
        }
      }
      this.correctAnswers = correct;
      this.showResults = true;
    },
  },
};
</script>

<style scoped>
#main {
  font-family: "Open Sans", sans-serif;
}
td.last {
  width: 1px;
  white-space: nowrap;
}
td.fitwidth {
  width: 1px;
  white-space: nowrap;
}
table {
  line-height: 3em;
}
.small {
  font-size: 7pt;
  color: grey;
}
.small a {
  color: grey;
  text-decoration: none;
}
@media print {
  body *,
  #main * {
    display: none;
  }
  #main,
  #main #printarea,
  #main #printarea * {
    display: block;
  }
}
</style>