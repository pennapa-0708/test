<template>
      <form action="" class="from1">
        <b-row>
          <b-col>
            <label for="number">Number of Questions:</label><br />
            <input
              type="number"
              class="number_questions"
              min="1"
              max="50"
              v-model="amount"
            /><br />
          </b-col>
        </b-row>
        <b-row>
          <b-col>
            <label for="catagory">Select Category:</label><br />
            <select
              name="catagory"
              id="catagory"
              class="select_catagory"
              v-model="category"
            >
              <option
                v-for="option_category in option_categorys"
                v-bind:value="option_category.value"
              >
                {{ option_category.text }}
              </option>
            </select>
          </b-col>
        </b-row>
        <b-row>
          <b-col>
            <b-button class="bt" v-on:click="generate">GENERATE QUIZ</b-button>
          </b-col>
        </b-row>
      </form>
</template>
<script>
import axios from 'axios'
export default {
props: {
    
  },
  data() {
    return {
      option_categorys: [
        { value: '0', text: 'Any Catagory' },
        { value: '9', text: 'General Knowledge' },
        { value: '10', text: 'Entertainment: Books' },
        { value: '11', text: 'Entertainment: Film' },
        { value: '12', text: 'Entertainment: Music' },
        { value: '13', text: 'Entertainment: Musicals & Theatres' },
        { value: '14', text: 'Entertainment: Television' },
        { value: '15', text: 'Entertainment: Video Games' },
        { value: '16', text: 'Entertainment: Board Games' },
        { value: '17', text: 'Science & Nature' },
        { value: '18', text: 'Science: Computers' },
        { value: '19', text: 'Science: Mathematics' },
        { value: '20', text: 'Mythology' },
        { value: '21', text: 'Sports' },
        { value: '22', text: 'Geography' },
        { value: '23', text: 'History' },
        { value: '24', text: 'Politics' },
        { value: '25', text: 'Art' },
        { value: '26', text: 'Celebrities' },
        { value: '27', text: 'Animals' },
        { value: '28', text: 'Vehicles' },
        { value: '29', text: 'Entertainment: Comics' },
        { value: '30', text: 'Science: Gadgets' },
        { value: '31', text: 'Entertainment: Japanese Anime & Manga' },
        { value: '32', text: 'Entertainment: Cartoon & Animations' },
      ],
      questionList: [],
      amount: 10,
      category: 0,
      amountCorrect: 0,
      amountInCorrect: 0,
      numberChoice: 1,
      stateGame: 0,
      choiceChoose: 0,
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
    getquestionList: function () {
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

        this.getquestion()
        this.stateGame = 1
        console.log(url)
        console.log(response)
        console.log(this.category)
      })
    },
    getquestion: function () {
      let result = this.questionList[this.numberChoice - 1]

      this.question.title = this.numberChoice + '. ' + result.question
      let choiceList = result.incorrect_answers
      choiceList.push(result.correct_answer)
      this.randomChoice(choiceList)
      this.question.answer = result.correct_answer
    },
    randomChoice: function (choiceList) {
      let choiceTempList = []

      let index = 1
      while (choiceList.length != 0) {
        let random = Math.floor(Math.random() * choiceList.length)
        if (index == 1) {
          this.question.choice1 = choiceList[random]
        }
        if (index == 2) {
          this.question.choice2 = choiceList[random]
        }
        if (index == 3) {
          this.question.choice3 = choiceList[random]
        }
        if (index == 4) {
          this.question.choice4 = choiceList[random]
        }
        index++
        choiceList.splice(random, 1)
        console.log('random' + random)
        console.log(choiceList)
      }
      console.log(choiceTempList)
      console.log(choiceList)
    },
    selectChoice: function (index) {
      if (this.choiceChoose == 0) {
        if (index == 1) {
          this.addScore(this.question.choice1)
        }
        if (index == 2) {
          this.addScore(this.question.choice2)
        }
        if (index == 3) {
          this.addScore(this.question.choice3)
        }
        if (index == 4) {
          this.addScore(this.question.choice4)
        }
        this.choiceChoose = index
        console.log(this.question)
      }
    },
    nextQuestion: function () {
      if (this.numberChoice == this.questionList.length) {
        this.stateGame = 2
      } else {
        this.choiceChoose = 0
        this.numberChoice++
        this.getquestion()
      }
    },
    addScore: function (choiceText) {
      if (choiceText == this.question.answer) {
        this.amountCorrect++
      } else {
        this.amountInCorrect++
      }
    },
    generate: function (event) {
      this.getquestionList()
    },
    resetQuestionList() {
      this.stateGame = 0
      this.choiceChoose = 0
      this.questionList = []
      this.amountCorrect = 0
      this.amountInCorrect = 0
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
label {
  color: white;
  font-weight: bold;
  margin-top: 1%;
}
input {
  width: 100%;
  height: 40px;
  border: none;
  padding-left: 1%;
}
select {
  width: 100%;
  height: 40px;
  border: none;
  padding-left: 1%;
}
.bt {
  margin-top: 2%;
  margin-bottom: 2%;
  width: 100%;
  height: 60px;
  font-size: 25px;
  font-weight: bold;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.6);
  border: none;
}
</style>
