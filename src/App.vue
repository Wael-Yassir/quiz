<template>
  <div id="app">
    <Header-Item
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <Question-Box
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increament="increament"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>

</template>

<script>
import HeaderItem from './components/Header-Item.vue'
import QuestionBox from './components/Question-Box.vue'

export default {
  name: 'App',
  components: {
    HeaderItem,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      if (this.index <= this.questions.length)
        this.index++
    },
    increament(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }

      this.numTotal++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=21&type=multiple', {
      method: 'get'
    })
      .then((res) => {
        return res.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results
      })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
