<template>
    <div class="questions-ctr">
      <div class="progress">
        <div class="bar"></div>
        <div class="status">
          {{ currentIndex + 1 }} out of {{ questions.length || 0 }} questions answered
        </div>
      </div>
      <div class="single-question">
        <div class="question" >{{ currentQuestion }}</div>
  
        <div class="answers" v-for="(answer, index) in answers" :key="answer">
          <div class="answer" :class="index === selectedAnswerIndex? 'blink' : ''" @click="captureAnswer(answer)">{{ answer }}</div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: 'Questions',
    data() {
      return {
        selectedAnswer: '',
        currentIndex: 0,
        selectedAnswerIndex: null

      }
    },
    props: {
      questions: {
        type: Array,
        required: true
      }
    },
    methods: {
      shuffleArray(array) {
        if (!array) return;  
        for (let i = array.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1))
          ;[array[i], array[j]] = [array[j], array[i]]
        }
        return array
      },
      captureAnswer(answer) {
        this.selectedAnswer = answer
        this.selectedAnswerIndex = this.answers.indexOf(answer)
        console.log('Selected answer:', this.selectedAnswer)
      }


    },
    watch: {
    //   selectedAnswer() {
    //     if (this.selectedAnswer) {
    //       this.currentIndex++
    //       this.selectedAnswer = ''
    //     //   this.$nextTick(() => {
    //     //     if (this.currentIndex === this.questions.length) {
    //     //       console.log('All questions answered')
    //     //     }
    //     //   })
    //     }
    //   }
    },
    computed: {
      currentQuestion() {
        if (!this.questions || !this.questions.length) return ''; 
        const question = this.questions[this.currentIndex]?.question || '';
        return question.replace(/(&quot;)/g,"\"")
      },
      answers() {
        if (!this.questions || !this.questions.length) return [];  
        const currentQuestionObj = this.questions[this.currentIndex];
        if (!currentQuestionObj || !currentQuestionObj.incorrect_answers || !currentQuestionObj.correct_answer) return [];
        const answersList = [
          ...currentQuestionObj.incorrect_answers,
          currentQuestionObj.correct_answer
        ];
        return this.shuffleArray(answersList) || [];
      }
    }
  }
  </script>
  
<style>
@keyframes blink {
  0%, 100% { background-color: #4fb357; } /* Initial and final state */
  50% { background-color: #ffff; } /* Intermediate state */
}

.blink {
  animation: blink 0.5s 3;
  background-color: #4fb357;
}

</style>
  