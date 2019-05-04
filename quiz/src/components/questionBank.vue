<template>
<div>
<b-jumbotron class="question-box-container">

    <div slot="lead">
    {{ createQuestion.question}}
    </div>
    <hr class="my-4">
    <b-list-group>
  <b-list-group-item v-for="(answer,index) in answers" 
                     :key="index" 
                     @click="selection(index)" 
                     :class = "answerClass(index)">{{ answer }}</b-list-group-item>
    </b-list-group>
    <b-button variant="primary" @click="submit" :disabled="selectedIndex=== null || answered">Submit</b-button>
    <b-button  @click="next" 
    :disabled="!answered "
    variant="success">Next</b-button>
</b-jumbotron>
</div>

</template>

<script>
    import _ from 'lodash'
export default {
    
    props: {
        createQuestion: Object,
        next: Function,
        increment: Function
    },
    data(){
      return{
          selectedIndex : null,
          suffleAnswer : [],
          correctIndex:null,
          answered : false
      }  
    },
    watch:{
        createQuestion:{
            immediate : true,
            handler(){
            
            this.selectedIndex = null,
            this.correctIndex = null,
            this.answered = false,
            this.suffle()
            }
        }
    },
    methods:{
        selection(index){
            this.selectedIndex = index
        },
        suffle(){
            let answers=[...this.createQuestion.incorrect_answers, this.createQuestion.correct_answer]
            this.suffleAnswer = _.shuffle(answers)
            this.correctIndex=this.suffleAnswer.indexOf(this.createQuestion.correct_answer)
    },
        submit(){
            let isCorrect = false
            
            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
        answerClass(index){
        let answersClass =''
        if(!this.answered && this.selectedIndex===index){
            answersClass ='selected'
        }
        else if(this.answered && this.correctIndex === index){
           answersClass = 'correct'
        }
        else if(this.answered && this.selectedIndex===index && this.correctIndex !== index){
           answersClass = 'incorrect'
        }
        return answersClass
    }
    },

    computed: {
        answers(){
            let answers=[...this.createQuestion.incorrect_answers,this.createQuestion.correct_answer]
            return answers
        }
    }
}


</script>

<style>
    .list-group{
        margin-bottom: 10px;
    }
    .btn{
        margin: 0 5px;
    }
    .list-group-item:hover{
        background: #EEE;
        cursor: pointer;
    }
    .selected{
        background-color: aqua;
    }
    .incorrect{
        background-color: lightcoral;
    }
    .correct{
        background-color: aquamarine;
    }

</style>