<template>
  <div class="question-box-conatainer">
    <b-jumbotron>
      <template #lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<style scoped>
.question-box-conatainer {
  margin-top: 50px;
}
</style>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increament: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  watch: {
    // this object allows to watch for a change for a props, it will run when it changes

    // to run the function when the currentQuestion is passed, you can use an object instead of function!
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
        this.answered = false;
      },
    },
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer); // push return the new lenght of the array!
      return answers;
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      let isCorrect = false;

      if (
        this.shuffledAnswers[this.selectedIndex] ===
        this.currentQuestion.correct_answer
      ) {
        isCorrect = true;
      }
      this.answered = true;
      this.increament(isCorrect);
    },
    answerClass(index) {
      let answerClass = ''

      if (!this.answered && this.selectedIndex === index)
        answerClass = 'selected'
      else if (this.answered && this.correctIndex === index)
        answerClass = 'correct'
      else if (this.answered && this.correctIndex !== index && this.selectedIndex === index)
        answerClass = 'incorrect'

      return answerClass;
    }
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background: lightblue;
}

.correct {
  background: lightgreen;
}

.incorrect {
  background: lightcoral;
}
</style>

