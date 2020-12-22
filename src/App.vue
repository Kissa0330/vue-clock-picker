<template>
<div id="app">
  <time-picker/>
  <time-picker/>
</div>
</template>

<script>
import TimePicker from './components/TimePicker'
export default {
  components: {
    TimePicker
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

#app {
  color: #2c3e50;
  margin-top: -100px;
  max-width: 600px;
  font-family: "Roboto", sans-serif;
  text-align: center;
  display: flex;
  justify-content: space-between;
  width: 400px;
}

#app a {
  color: #42b983;
  text-decoration: none;
}
</style>
