<template>
  <div id="app" class="container">
    <img src="./assets/gsd_logo.png" class="logo">
    <task-list></task-list>
  </div>
</template>

<script>
import TaskList from './components/TaskList'

export default {
  name: 'App',
  components: {
    TaskList,
  }
}
</script>

<style>

* {box-sizing: border-box;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}
.logo {
  display: block;
  margin: 20px auto;
  height: 140px;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}
</style>
