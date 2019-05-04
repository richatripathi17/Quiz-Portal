<template>
  <div id="app">
  <div v-if="totalNum === 10">
      <b-nav tabs>
          <b-nav-item disabled><h1><b>QUIZ</b></h1></b-nav-item>
  </b-nav>
  <b-col sm="6" offset="3">
        <b-jumbotron>
            <h2>Total  : {{ correctNum }} / {{ totalNum }}</h2>
          </b-jumbotron>
     </b-col>
  </div>
  <div v-else>
     <Header
    :numCorrect="correctNum"
    :numTotal="totalNum"         />
      
      <b-container class="bv-example-row">
  <b-row>
    <b-col sm="6" offset="3"><questionBox v-if="questions.length"              
                            :createQuestion="questions[index]"
                            :next="next"
                            :increment="increment" 
                             />
    </b-col>
  </b-row> 
</b-container>
      
     </div>
  </div>
</template>

<script>
import Header from './components/header.vue'
import questionBox from './components/questionBank.vue'


export default {
  name: 'app',
  components: {
    Header ,
    questionBox
  },
    data(){
        return{
      questions: [],
      index: 0,
      correctNum: 0,
      totalNum: 0
        }
    },
    methods: {
        next(){
            this.index++
        },
        increment(isCorrect){
            if(isCorrect){
                this.correctNum++
            }
            this.totalNum++
        }
    },
  mounted : function(){
      fetch('https://opentdb.com/api.php?amount=10&category=18&type=multiple',{
      method: 'get'
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
