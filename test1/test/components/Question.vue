<template>
  <b-container>
    <b-row>
      <b-col>
        <p class="urlapi text-light" v-html="question.title"></p>
        <div
          class="choice"
          :class="{
            'bg-danger':
              choiceChoose == 1 && question.choice1 != question.answer,
            'bg-success':
              choiceChoose != 0 && question.choice1 == question.answer,
          }"
          @click="selectChoice(1)"
        >
          <p class="choice-prefix">A</p>
          <p class="choice-text" v-html="question.choice1"></p>
        </div>
        <div
          class="choice"
          :class="{
            'bg-danger':
              choiceChoose == 2 && question.choice2 != question.answer,
            'bg-success':
              choiceChoose != 0 && question.choice2 == question.answer,
          }"
          @click="selectChoice(2)"
        >
          <p class="choice-prefix">B</p>
          <p class="choice-text" v-html="question.choice2"></p>
        </div>
        <div
          class="choice"
          :class="{
            'bg-danger':
              choiceChoose == 3 && question.choice3 != question.answer,
            'bg-success':
              choiceChoose != 0 && question.choice3 == question.answer,
          }"
          @click="selectChoice(3)"
        >
          <p class="choice-prefix">C</p>
          <p class="choice-text" v-html="question.choice3"></p>
        </div>
        <div
          class="choice"
          :class="{
            'bg-danger':
              choiceChoose == 4 && question.choice4 != question.answer,
            'bg-success':
              choiceChoose != 0 && question.choice4 == question.answer,
          }"
          @click="selectChoice(4)"
        >
          <p class="choice-prefix">D</p>
          <p class="choice-text" v-html="question.choice4"></p>
        </div>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        <b-button
          class="btquiz"
          @click="nextQuestion"
          :disabled="choiceChoose == 0"
          >nextquestion</b-button
        >
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
export default {
  props: {
    questionList: Array,
    amount: Number,
  },
  data() {
    return {
      numberChoice: 1,
      choiceChoose: 0,
      countCorrect: 0,
      countInCorrect: 0,
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
  created() {
    this.getQuestion()
  },
  methods: {
    getQuestion: function () {
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
        } else if (index == 2) {
          this.question.choice2 = choiceList[random]
        } else if (index == 3) {
          this.question.choice3 = choiceList[random]
        } else if (index == 4) {
          this.question.choice4 = choiceList[random]
        }
        index++
        choiceList.splice(random, 1)
      }
    },
    selectChoice: function (index) {
      if (this.choiceChoose == 0) {
        if (index == 1) {
          this.addScore(this.question.choice1)
        } else if (index == 2) {
          this.addScore(this.question.choice2)
        } else if (index == 3) {
          this.addScore(this.question.choice3)
        } else if (index == 4) {
          this.addScore(this.question.choice4)
        }
        this.choiceChoose = index
      }
    },
    addScore: function (choiceText, countCorrect, countInCorrect) {
      if (choiceText == this.question.answer) {
        this.countCorrect++
      } else {
        this.countInCorrect++
      }
      this.$emit('score', this.countCorrect, this.countInCorrect)
    },
    nextQuestion: function () {
      if (this.numberChoice == this.questionList.length) {
        this.$emit('changeState', 3)
      } else {
        this.choiceChoose = 0
        this.numberChoice++
        this.getQuestion()
      }
    },
  },
}
</script>
<style>
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
</style>
