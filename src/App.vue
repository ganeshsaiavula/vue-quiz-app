<template>
  <div id="app">
    <Header :numCorrectAnswers="numCorrectAnswers"
            :numTotal="numTotal"/>
    <b-container class="bv-example-row">
  <b-row>
    <b-col sm="6" offset="3">
    <QuestionBox 
    v-if="questions.length"
    :currentQuestion="questions[index]"
    :next="next"
    :increment="increment"
    :ended="ended"/>      
    </b-col>
  </b-row>
</b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index:0,
      numCorrectAnswers:0,
      numTotal:0,
      ended: false,
    }
  },
  methods: {
    next() {
      if( this.questions.length-1 > this.index){
        console.log("Here")
        return this.index++ 
      }
      this.ended = true
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrectAnswers++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=25&type=multiple',{
      method:'get'
    })
    .then((response)=>{
      return response.json()
    })
    .then((jsonData)=>{
      this.questions = jsonData.results
    })
  }
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
</style>
