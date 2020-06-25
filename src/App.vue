<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal"/>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="12">
          <QuestionBox v-if="questions.length" v-bind:currentQuestion="questions[index]" :next="next" :increment="increment" />
        </b-col>
      </b-row>
    </b-container>
    
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

  export default {
    name: 'App',
    components: {
      Header,
      QuestionBox
    },
    data(){
      return{
        questions: [],
        index:0,
        numCorrect : 0,
        numTotal: 0

      }
    },
    methods:{
      next(){
        // console.log(this.index)
        this.index++
        if(this.numTotal <=9){
          this.numTotal++
        }
      },

      increment(isCorrect){
        if (isCorrect){
          this.numCorrect++
        }
        this.numTotal++
        console.log(this.numTotal+"ss"+ isCorrect)
      }
    },
    mounted:function(){
      fetch('https://opentdb.com/api.php?amount=10&category=23&difficulty=medium&type=multiple',{
        method: 'get'
      })
      .then(response=> response.json())
      .then((jsonData) =>{
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
