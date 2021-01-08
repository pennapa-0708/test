<template>
  <b-container>
    <Title title="Web quiz" />
    <!-- <section2/> -->
    <b-row v-if="stateGame == 2">
      <b-col>
        <div class="amount">score {{ countCorrect }}/{{ amount }}</div>
      </b-col>
    </b-row>
    <Start v-if="stateGame == 1" v-on:getQuestionList="getQuestionList" />
    <Question
      v-if="stateGame == 2"
      :questionList="questionList"
      :amount="amount"
      @changeState="changeState"
      @score="score"
    />
    <Result
      v-if="stateGame == 3"
      :countCorrect="countCorrect"
      :countInCorrect="countInCorrect"
      @resetQuestionList="resetQuestionList"
    />
  </b-container>
</template>
<script>
import axios from 'axios'
import Title from '@/components/Title'
import Start from '@/components/Start'
import Question from '@/components/Question'
import Result from '@/components/Result'
export default {
  components: {
    Title,
    Start,
    Question,
    Result,
  },
  data() {
    return {
      questionList: [],
      amount: 10,
      countCorrect: 0,
      countInCorrect: 0,
      stateGame: 1,
    }
  },
  methods: {
    changeState: function (state) {
      this.stateGame = state
    },
    getQuestionList: function (category, amount) {
      this.amount = amount
      this.category = category
      const url = new URL('https://opentdb.com/api.php')
      url.searchParams.append('amount', this.amount)
      url.searchParams.append('category', this.category)
      url.searchParams.append('type', 'multiple')
      axios.get(url).then((response) => {
        this.questionList = response.data.results
        this.stateGame = 2
      })
    },
    score: function (countCorrect,countInCorrect) {
     this.countCorrect = countCorrect
     this.countInCorrect= countInCorrect
    },
    resetQuestionList() {
      this.stateGame = 1
      this.choiceChoose = 0
      this.questionList = []
      this.countCorrect = 0
      this.countInCorrect = 0
      this.numberChoice = 1
    },
  },
}
</script>
<style>
body {
  background-color: #383e4e;
}
.amount {
  float: right;
  color: white;
  font-weight: bold;
  border: 2px solid black;
  padding: 1%;
}
</style>
