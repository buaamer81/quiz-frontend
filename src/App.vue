<template>
  <div id="app">
    <header>
      <h1>My Quiz Application</h1>
    </header>
    <main>
      <!-- Use the Quiz component -->
      <QuizPage />
    </main>
  </div>
</template>

<script>
import QuizPage from './components/QuizPage.vue';

export default {
  name: 'App',
  components: {
    QuizPage
  }
};
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
