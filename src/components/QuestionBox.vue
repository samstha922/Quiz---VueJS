<template>
	<b-jumbotron>
		<template v-slot:lead>
			{{currentQuestion.question}}
		</template>

		<hr class="my-4">
		<b-list-group class="list-group">
			<!-- key value pair is opposite in below -->
			<b-list-group-item  v-for="(answer,index) in answers" :key="index" @click="selectAnswer(index)" :class="answerClass(index)">{{answer}} 
			</b-list-group-item>
		</b-list-group>
		
		<b-button variant="primary" :disabled="selectedIndex === null || answered" v-on:click="submitAnswer" href="#">
			Submit
		</b-button>
		<b-button @click="next" variant="success" href="#">Next</b-button>
	</b-jumbotron>
</template>

<style scoped>
	.list-group{
		margin-bottom: 15px;
	}
	.list-group-item:hover{
		background: #eee;
		cursor: pointer;
	}
	.selected{
		background: blue;
	}
	.correct{
		background: green;
	}
	.incorrect{
		background: red;
	}
</style>

<script>
	import _ from 'lodash';    

	export default{

		props:{
			currentQuestion: {},
			next: Function,
			increment: Function,
		},

		mounted(){
			//to shuffle it on the first question else first won't be shuffled
			this.shuffleAnswers()
			//console.log(this.currentQuestion);
		},

		data(){
			return{
				selectedIndex: null,
				correctIndex: null,
				shuffledAnswers: null,
				answered: false	
			}
		},

		methods:{
			selectAnswer(index){
				this.selectedIndex = index 
				//console.log(index);
			},
			shuffleAnswers(){
				// loadash helper library to shuffle things
				let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
				this.shuffledAnswers= _.shuffle(answers)
				this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
			},

			submitAnswer(){
				let isCorrect= false
				if(this.selectedIndex == this.correctIndex){
					isCorrect = true
				}
				this.answered = true
				this.increment(isCorrect)
			},
			// to find the correct and incorrect answer
			answerClass(index){
				let answerClass = ''
				if(!this.answered && this.selectedIndex == index ){
					answerClass='selected'
				} else if (this.answered && this.correctIndex === index){
					answerClass = 'correct'
				} else if (this.answered && this.selectedIndex == index && this.correctIndex !== index){
					answerClass = 'incorrect'
				}
				return answerClass	
			}
		},
		//takes obj of fxn ....can watch changes to the props....needs same name as prop
		watch:{
			// //currentQuestion is a function
			// curr entQuestion(){
			// 	this.selectedIndex = null
			// 	this.shuffleAnswers()
			// }

			currentQuestion:{
				immediate: true,
				handler(){
					this.selectedIndex = null
					this.answered= false
					this.shuffleAnswers()
				}
			}
		},
		computed:{
			//// initially this method was used when not shuffled; now the answers has been placed in the watch function....still it can't be commented since the answers won't be loaded if deleted
			answers(){
				let answers = [...this.currentQuestion.incorrect_answers]
				//console.log(answers);
				answers.push(this.currentQuestion.correct_answer)
				return answers
			}
		}
	}
</script>

