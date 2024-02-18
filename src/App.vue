<template>
  <div class="ctr">
    <Questions :questions="QuestionsData" />

    <div class="result">
    <div class="title">You got sample result 1!</div>
    <div class="navigation">
           <button type="button" class="navigation-btn">Previous</button>
           <button type="button" class="navigation-btn">Next</button>
    </div>

    <button type="button" class="reset-btn">Reset</button>
  </div>
</div>
</template>

<script>
import Questions from './components/Questions.vue'
import Result from './components/Result.vue'

export default {
  name: 'App',
  data() {
    return {
      QuestionsData: []
    }
  },
  methods: {
    async fetchTrivia() {
      try {
        const response = await fetch(
          'https://opentdb.com/api.php?amount=10&category=9&difficulty=easy&type=multiple'
        )
        const data = await response.json()
        this.QuestionsData = data.results
      } catch (error) {
        console.error(error)
      }
    },
  },
  created() {
    this.fetchTrivia()
    console.log(this.QuestionsData)
  },

  components: {
    Questions,
    Result
  }
}
</script>

<style>
.navigation {
  display: flex;
  justify-content: space-between;
  height: fit-content;
  background-color:#524d4d;
}
</style>
