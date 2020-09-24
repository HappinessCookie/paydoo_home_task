<template>
  <div id="app">
    <h1>Currency Conversion Tool</h1>
    <div class="row">
      <div class="col">
        <h2>From USD</h2>
        <label>
          <input class="input" :value="usdValue" type="number" min="0" @input="usdValueChange">
        </label>
      </div>
      <div class="col">
        <h2>
          <span>To </span>
          <label for="currencySelector">
            <select class="select" id="currencySelector" v-model="selectedRate">
              <option v-if="ratesList.length === 0" :value="null">Choose currency</option>
              <option v-for="rate in ratesList" :key="rate" :value="rate">{{ rate }}</option>
            </select>
          </label>
        </h2>
        <label>
          <input class="input" :value="customCurrencyValue" type="number" min="0" @input="customCurrencyValueChange">
        </label>
      </div>
    </div>
  </div>
</template>

<script>
import {Decimal} from "decimal.js";

const appId = "0e99a90e26224cc285f0d8f8cc7cadf2";
const currencyRatesUrl = `https://openexchangerates.org/api/latest.json?app_id=${appId}&base=USD`;

export default {
  name: 'App',
  data() {
    return {
      selectedRate: null,
      usdValue: 0,
      customCurrencyValue: 0,
      rates: {},
    };
  },
  computed: {
    ratesList() {
      return Object.keys(this.rates);
    },
    currentRateMultiply() {
      return this.rates[this.selectedRate] || null;
    },
  },
  watch: {
    selectedRate() {
      this.customCurrencyValueCalculate();
    },
  },
  methods: {
    async loadRates() {
      const {rates} = await fetch(currencyRatesUrl).then(response => response.json());
      this.rates = rates;
      [this.selectedRate] = this.ratesList;
    },
    multiplyCurrency(from, to) {
      return new Decimal(from).times(to).toFixed(2)
    },
    customCurrencyValueCalculate() {
      this.customCurrencyValue = this.multiplyCurrency(this.usdValue, this.currentRateMultiply);
    },
    usdValueCalculate() {
      this.usdValue = this.multiplyCurrency(this.customCurrencyValue, this.currentRateMultiply);
    },
    usdValueChange($event) {
      if (this.currentRateMultiply === null) {
        return;
      }
      this.usdValue = $event.target.value;
      this.customCurrencyValueCalculate();
    },
    customCurrencyValueChange($event) {
      if (!this.currentRateMultiply) {
        return;
      }
      this.customCurrencyValue = $event.target.value;
      this.usdValueCalculate();
    },
  },
  created() {
    this.loadRates();
  },
};
</script>

<style lang="scss">
html, body {
  margin: 0;
}

body {
  display: flex;
  align-content: center;
  justify-content: center;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.row {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 10px;
}

.input {
  font-size: 1rem;
  text-align: center;
}

h2, .select {
  font-size: 1.5rem;
  font-weight: bold;
}

.input, .select {
  border: 0;
  color: inherit;
  box-shadow: 0 0 0 1px currentColor;
  outline: none;
}

.input {
  padding: 5px;
}
</style>
