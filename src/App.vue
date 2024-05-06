<template>
  <div>
    <div class="logo-container">
      <img src="/logo-512x512.png" alt="Logo" class="logo">
    </div>
    <ShowIntro />
    <ShowQuiz />
  </div>

</template>

<script>
import ShowIntro from './components/ShowIntro.vue'
import ShowQuiz from './components/ShowQuiz.vue'

export default {
  name: 'App',
  components: {
    ShowIntro,
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

.logo {
  width: 15vw; /* Adjust as needed */
  height: 15vw; /* Adjust as needed */
  margin-top: 2vw; /* Adjust as needed */
}
</style>
