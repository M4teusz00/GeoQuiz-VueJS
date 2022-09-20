<script setup>
import { ref, computed} from 'vue'

const questions = ref([
	{
		question: '1. Which mountain is the highest?',
		answer: 0,
		options: [
			'Mount Everest',
			'Kilimandżaro',
			'Mount Blanc',
			'K2'
		],
		selected: null
	},
	{
		question: '2. The longest river in Europe is:',
		answer: 1,
		options: [
			'Dunaj',
			'Wołga',
			'Wisła', 
			'Sekwana'
		],
		selected: null
	},
	{
		question: '3. France does not border:',
		answer: 0,
		options: [
			'Netherlands',
			'Germany',
			'Italy',
			'Spain'
		],
		selected: null
	},
	{
		question: '4. The smallest country in the world is:',
		answer: 2,
		options: [
			'Luxemburg',
			'Andorra',
			'Vatican',
			'Monaco'
		],
		selected: null
	},
	{
		question: '5. Which of the oceans is the largest?',
		answer: 3,
		options: [
			'Atlantic',
			'Indian',
			'Arctic',
			'Pacific'
		],
		selected: null
	},
	{
		question: '6. The Great Britain does not include:',
		answer: 2,
		options: [
			'Wales',
			'Scotland',
			'Iceland',
			'England'
		],
		selected: null
	},
	{
		question: '7. Are there more than 150 countries in the world?',
		answer: 0,
		options: [
			'Yes',
			'No'
		],
		selected: null
	},
	{
		question: '8. Greenland is dependent on the country:',
		answer: 1,
		options: [
			'Sweden',
			'Denmark', 
			'The United Kingdom',
			'USA'
		],
		selected: null
	},


])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected == q.answer) {
			value++
		}
	})
	return value
})

const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const SetAnswer = (evt) => {
	questions.value[currentQuestion.value].selected = evt.target.value
	evt.target.value = null
}

const NextQuestion = () => {
	if(currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++	
		return	
	} 
	quizCompleted.value = true
	
}

</script>

<template>
	<main class="app">
		<h1>The Geographic Quiz</h1>
		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score {{ score }} / {{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
					:key="index"
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer
								? 'correct'
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:name="getCurrentQuestion.index"
						:value="index"
						v-model="getCurrentQuestion.selected"
						:disabled="getCurrentQuestion.selected"
						@change="SetAnswer">
					<span>{{ option }}</span>
				</label>
			</div>

			<button
				@click="NextQuestion"
				:disabled="!getCurrentQuestion.selected">
				{{	
					getCurrentQuestion.index == questions.length - 1
						? 'Finish'
						: getCurrentQuestion.selected == null
							? 'Select an option'
							: 'Next Question'
				}}
			</button>
		</section>
		<section v-else>
			<h2>You have finished the quiz!</h2>
			<p>Your score is {{ score }} / {{ questions.length }}</p>
		</section>

	</main>
	
</template>
