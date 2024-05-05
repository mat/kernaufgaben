<template>
  <ShowIntro />
  <MultiplicationTable />
  <ShowQuiz />
</template>

<script>
import ShowIntro from './components/ShowIntro.vue'
import MultiplicationTable from './components/MultiplicationTable.vue'
import ShowQuiz from './components/ShowQuiz.vue'

export default {
  name: 'App',
  components: {
    ShowIntro,
    MultiplicationTable,
    ShowQuiz
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
