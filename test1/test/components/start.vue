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
export default {
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
    }
  },
  methods: {
    generate: function () {
      this.$emit('getquestionList', this.category, this.amount)
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
