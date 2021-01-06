<template>
  <b-container>
    <b-row>
      <b-col
        ><div class="title"><h1>{{title}}</h1></div></b-col>
    </b-row>
  </b-container>
</template>
<script>
export default {
  props: {
    title: String,
  },
}
</script>
<style>
h1{
  text-align: center;
  padding: 2%;
  margin-top: 2%;
  color: white;
}
</style>
