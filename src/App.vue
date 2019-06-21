<template>
  <div id="app">
    <Header
    :correct="numCorrect"
    :total="numTotal"
    ></Header>
    <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
                 :next="next"
            :increment="increment"
    ></QuestionBox>

  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'


export default {
  name: 'app',
  data(){
    return {
      questions:[],
      index:0,
      numCorrect:0,
      numTotal:0
    }
  },
  methods:{
    next(){
      this.index++
    },
    increment(isCorrect){
     if(isCorrect){
       this.numCorrect++
     }
     this.numTotal++
    }
  },
  components: {
    Header,
    QuestionBox
  },
  mounted: function(){

    fetch('https://opentdb.com/api.php?amount=10&category=10&type=multiple',{
      method:'get'
    })
            .then((response) => {
              return response.json()
            })
            .then((jsonData) => {
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
