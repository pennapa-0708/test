<template>
  <b-container>
    <Title title="Web quiz" />
    <!-- <section2/> -->
    <b-row v-if="stateGame == 2">
      <b-col>
        <div class="amount">score {{ countCorrect }}/{{ amount }}</div>
        <div class="text-light">
          {{ question.answer }}
        </div>
      </b-col>
    </b-row>
    <Start v-if="stateGame == 1" v-on:getquestionList="getquestionList" />
    <Question
      v-if="stateGame == 2"
      :questionList="questionList"
      :amount="amount"
      v-on:changeState="changeState"
      v-on:addScore="addScore"
    />
    <Reset
      v-if="stateGame == 3"
      :countCorrect="countCorrect"
      :countInCorrect="countInCorrect"
      v-on:resetQuestionList="resetQuestionList"
    />
  </b-container>
</template>
<script>
import axios from 'axios'
import Title from '@/components/Title'
import Start from '@/components/Start'
import Question from '@/components/Question'
import Reset from '@/components/Reset'
export default {
  components: {
    Title,
    Start,
    Question,
    Reset,
  },
  data() {
    return {
      questionList: [],
      amount: 10,
      countCorrect: 0,
      countInCorrect: 0,
      stateGame: 1,
      question: {
        title: '',
        choice1: '',
        choice2: '',
        choice3: '',
        choice4: '',
        answer: '',
      },
    }
  },
  methods: {
    changeState: function (state) {
      this.stateGame = state
    },
    getquestionList: function (category, amount) {
      this.amount = amount
      this.category = category
      let apiquestions = 'https://opentdb.com/api.php?'
      console.log('category', this.category)
      let url = ''
      if (this.category == 0) {
        url = apiquestions + 'amount=' + this.amount + '&type=multiple'
      } else {
        url =
          apiquestions +
          'amount=' +
          this.amount +
          '&category=' +
          this.category +
          '&type=multiple'
      }
      axios.get(url).then((response) => {
        this.questionList = response.data.results
        // this.getquestion()
        this.stateGame = 2
        console.log(url)
        console.log(response)
        console.log(this.category)
      })
    },
    addScore: function ({ choiceText, answer }) {
      if (choiceText == answer) {
        this.countCorrect++
      } else {
        this.countInCorrect++
      }
    },
    resetQuestionList() {
      this.stateGame = 1
      this.choiceChoose = 0
      this.questionList = []
      this.countCorrect = 0
      this.countInCorrect = 0
      this.numberChoice = 1
      this.question = {
        title: '',
        choice1: '',
        choice2: '',
        choice3: '',
        choice4: '',
        answer: '',
      }
    },
  },
}
</script>
<style>
body {
  background-color: #383e4e;
}
</style>
