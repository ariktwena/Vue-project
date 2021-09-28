<template>
  <div id="app">
    <Header
        :numCorrect = "numCorrect"
        :numTotal = "numTotal"
    />

    <br>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3" class="question-box">
          <div v-if='this.index < 10'>
            <QuestionBox
                v-if="questions.length > 0"
                :currentQuestion = "questions[index]"
                :nextQuestion = "next"
                :increment = "increment"
            />
          </div>
          <div v-if='this.index >= 10'>
            <p>Game Over</p>
          </div>
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from "@/components/QuestionBox";

export default {
  name: 'App',
  components: {
    Header,
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
    next(isAnswared) {
      console.log(isAnswared)
      if(!isAnswared){
        this.numTotal++
      }
      this.index++
    },
    increment(isCorrect) {
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function (){
    fetch('https://opentdb.com/api.php?amount=10&category=27&difficulty=easy&type=multiple', {
      method: 'get'
    })
        .then((response) => {
          // console.log(response.json())
          return response.json()
    })
        .then((jsonData) => {
          // console.log(jsonData.results)
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
.question-box {
  background: #EEE;
  padding-top: 10px;
  padding-bottom: 10px;
}
</style>
