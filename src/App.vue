<template>
  <div id="app">
    <persian-date-range-picker></persian-date-range-picker>
  </div>
</template>

<script>
import PersianDateRangePicker from "./components/PersianDateRangePicker/PersianDateRangePicker";

export default {
  name: 'app',
  components: {
    PersianDateRangePicker
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
