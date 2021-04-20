<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <Header msg="My to do list"/>
    <List/>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import List from './components/List.vue';

export default {
  name: 'App',
  components: {
    Header,
    List
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #5c5f61;
  width: 95%;
  margin: auto;
  margin-top: 60px;
}
</style>
