<template>
  <div>
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
        <b-button class="btn" @click="reset">Submit</b-button>
      </b-col>
    </b-row>
  </div>
</template>
<script>
export default {
  props: {
    countCorrect: Number,
    countInCorrect: Number,
  },
  methods: {
    reset: function () {
      this.$emit('resetQuestionList')
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
</style>
