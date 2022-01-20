<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item v-for="(answer, index) in answers" 
        :key="index"
        @click="selectAnswer(index)"
        :class="[selectedIndex === index? 'selected': '']">
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button @click="next" variant="success" href="#"> Next </b-button>
    </b-jumbotron>
  </div>
</template>


<script>
// Load the full build.
let _ = require('lodash');
export default {
  props: {
    currentQuestion: Object,
    next: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: []
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]; // copy the array
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    currentQuestion() {
      this.selectedIndex = null;
      this.shuffleAnswers();

    }
  },
  methods: {
    selectAnswer(index){
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]; // copy the array
      this.shuffledAnswers = _.shuffle(answers)
    }
  },
  mounted() {
    this.shuffleAnswers()
  },
};
</script>
<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #EEE;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
</style>
