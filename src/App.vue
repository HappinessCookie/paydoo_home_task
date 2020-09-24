<template>
  <div id="app">
    <h1>Currency Conversion Tool</h1>
    <label for="currencySelector">
      <select id="currencySelector">
        <option>Choose currency</option>
        <option v-for="rate in ratesList" :key="rate" :value="rate">{{rate}}</option>
      </select>
    </label>
    <label for="inputUsd">
      <span>USD</span>
      <input id="inputUsd" type="number" value="0">
    </label>
    <label for="inputCustomCurrency">
      <span id="currency2__name">USD</span>
      <input id="inputCustomCurrency" type="number" value="0">
    </label>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
