<script setup>
import {ref, computed, registerRuntimeCompiler} from 'vue'

const questions = ref([
  {
    question: 'What is Vue Js?',
    answer: 0,
    options:[
      'A frontend framework',
      'A framework for building web apps',
      'A framework for building mobile apps',
      'A framework for building desktop apps'
    ],
    selected: null
  },
  {
    question: 'What is Vuex?',
    answer: 2,
    options:[
      'A backend framework',
      'A framework for building web apps',
      'Vue with an X',
      'A state managment library'
    ],
    selected: null
  },
  {
    question: 'What is Vue Router used for?',
    answer: 1,
    options:[
      'Walking in space',
      'A routung library for Vue Js',
      'Set of classes',
      'A networking device'
    ],
    selected: null
  }
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() =>{
  let value = 0
  questions.value.map(q => {
    if (q.selected == q.answer){
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

const setAnswer = evt =>{
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const nextQuestion = () =>{
  if (currentQuestion.value < questions.value.length -1){
    currentQuestion.value++
  }else{
    quizCompleted.value = true
  }
}
</script>

<template>
  <main class="app">
    <h1>The Quiz</h1>

    <section class="quiz" v-if="!quizCompleted">
      <quiz class="info">
        <div>
        <span class="question">
          {{ getCurrentQuestion.question }}
        </span>
        <span class="score">
          {{ score }} / {{ questions.length }}
        </span>
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
        }${
          getCurrentQuestion.selected != null &&
          index != getCurrentQuestion.selected
          ? 'disabled'
          : ''
        }`">
          <input 
          type="radio" 
          :name="getCurrentQuestion.index"
          value="index"
          v-model="getCurrentQuestion.selected"
          :disabled="getCurrentQuestion.selected"
          @change="setAnswer">
          <span>{{ option }}</span>
        </label>
      </div>
      <button @click="nextQuestion"
      :disabled="!getCurrentQuestion.selected">
        {{ getCurrentQuestion.index == questions.length -1
        ? 'finish'
        : getCurrentQuestion.selected == null
        ? 'Select an option'
        : 'Next Question'}}
      </button>
      </quiz>
    </section>
    <section v-else>
      <h2>You have finished the quiz!</h2>
      <p>Your Score is {{ score }} / {{ questions.length }}</p>
  </section>
  </main>
</template>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'montserrat', sans-serif;
}
body{
  background-color: #271c36;
  color: #fff;

}
.app{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}
h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}
.quiz{
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}
.info{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;

}
.info .question{
  color: #8f8f8f;
  font-size: 1.25rem;

}
.info .score{
  color: #fff;
  font-size: 1.25rem;
}
.options{
  margin-bottom: 1rem;
}
.option{
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
.option:hover{
  background-color: #2d213f;
}
.option .correct{
  background-color: #2cce7d;
}
.option .wrong{
  background-color: #ff5a5f;
}
.option:last-of-type{
  margin-bottom: 0;
}
.option .disabled{
  opacity: 0.5;
}
.option input{
  display: none;
}
button{
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;

  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}
button:disabled{
  opacity: 0.5;
}
h2{
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p{
  color: #8f8f8f;
  font-size: 1.25rem;
  text-align: center;
}
</style>
