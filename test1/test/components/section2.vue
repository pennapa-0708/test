<template>
  <b-container>
    <!-- <template>
      <start :questionList="questionList" />
    </template> -->
    <template v-if="stateGame == 1">
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
    <template v-if="stateGame == 2">
      <Question
        :questionList="questionList"
        :countCorrect="countCorrect"
        :countInCorrect="countInCorrect"
        :amount="amount"
        v-on:changeState="changeState"
        @addScore="addScore"
      />
    </template>
    <template v-if="stateGame == 3">
      <div class="card">
        <b-row>
          <b-col>
            <h2 class="correct">Correct answer : {{ countCorrect }}</h2>
          </b-col>
        </b-row>
        <b-row>
          <b-col>
            <h2 class="incorrect">InCorrect answer : {{ countInCorrect }}</h2>
          </b-col>
        </b-row>
      </div>
      <b-row>
        <b-col>
          <b-button class="btn" @click="resetQuestionList">Submit</b-button>
        </b-col>
      </b-row>
    </template>
    <!-- <span class="urlapi"></span> -->
  </b-container>
</template>
<script>

import Question from './Question'
// import start from './start'
// import reset from './reset'
export default {
  components: {
    Question,
    // start,
    // reset
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
      countCorrect: 0,
      countInCorrect: 0,
      numberChoice: 1,
      stateGame: 1,
      choiceChoose: 0,
    }
  },
  methods: {
    changeState: function (state) {
      this.stateGame = state
    },
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

        // this.getquestion()
        this.stateGame = 2
        console.log(url)
        console.log(response)
        console.log(this.category)
      })
    },
    addScore: function ({choiceText,answer}) {
      if (choiceText == answer) {
        this.countCorrect++
      } else {
        this.countInCorrect++
      }
    },
    generate: function (event) {
      this.getquestionList()
    },
    resetQuestionList() {
      this.amount=10
      this.category=0
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
.correct {
  text-align: center;
  color: green;
}
.incorrect {
  text-align: center;
  color: red;
}
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
.choice {
  display: flex;
  margin-top: 2%;
  margin-bottom: 1%;
  width: 100%;
  border: 0.1px solid blue;
  background-color: white;
}
.choice:hover {
  cursor: pointer;
  background-color: #ffcccc;
}
.choice-prefix {
  padding: 2%;
  margin: 0;
  background-color: #ff99cc;
  color: white;
  text-align: center;
}
.choice-text {
  padding: 2%;
  height: 0px;
  text-align: center;
}
.urlapi {
  font-weight: bold;
  font-size: 25px;
}
.card {
  padding: 2%;
  background-color: #ccffff;
}
.btn {
  width: 100%;
  height: 50px;
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 2%;
}
.amount {
  float: right;
  color: white;
  font-weight: bold;
  border: 2px solid black;
  padding: 1%;
}
</style>
