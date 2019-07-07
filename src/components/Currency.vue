<template>
  <div id="app">
    <form v-on:submit.prevent = "getRate">
      <label>
        <select v-model="currency" class="currenciesList">
          <option disabled value="">Выберите валюту</option>
          <option v-for="currencyName in currencyNames">{{currencyName}}</option>
        </select>
      </label>
      <label>
        <input v-model.date="date" type="date" class="date" >
      </label>
      <input type="submit" class="submit">
    </form>
    <CurrencyTable v-if="Object.keys(currencyRate).length !==0" v-bind:rate="currencyRate"></CurrencyTable>
  </div>
</template>

<script>
  import CurrencyTable from './CurrencyTable'

  export default {
    name: "Currency",
    components: {
      CurrencyTable
    },
    data() {
      return {
        currencyNames: [],
        date: new Date(),
        currency: '',
        currencyRate: {}
      }
    },
    mounted() {
      fetch('https://www.nbrb.by/API/ExRates/Rates?Periodicity=0')
        .then((res) => res.json())
        .then((res) => {
          this.currencyNames = res.map(function (currency) {
            return currency.Cur_Abbreviation;
          })
        })
    },
    methods: {
      getRate() {
          fetch(`https://www.nbrb.by/API/ExRates/Rates/${this.currency}?ParamMode=2&onDate=${this.date}&Periodicity=0`)
            .then((res) => res.json())
            .then((res) => {
              this.currencyRate = res;
            })
      }
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
  .currenciesList {
    margin-right: 10px;
    height: 40px;
    padding: 5px;
    width: 210px;
    font-size: 20px;
    background: #AEBAFF;
    border: 2px solid #999;
    border-radius: 5px;
    box-shadow: 0 0 5px #999;
    color: white;
  }
  option {
    font-size: 18px;
  }
  .date {
    margin-right: 10px;
    padding: 7px;
    height: 22px;
    width: 200px;
    font-size: 20px;
    color: white;
    border: 2px solid #999;
    box-shadow: 0 0 5px #999;
    border-radius: 5px;
  }

  [type="date"] {
    background:#AEBAFF url(https://cdn1.iconfinder.com/data/icons/cc_mono_icon_set/blacks/16x16/calendar_2.png)  97% 50% no-repeat ;
  }

  [type="date"]::-webkit-inner-spin-button {
    display: none;
  }
  [type="date"]::-webkit-calendar-picker-indicator {
    opacity: 0;
  }

  input[type=submit] {
    padding:5px 15px;
    background: #AEBAFF;
    color: white;
    border:0 none;
    cursor:pointer;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    height: 40px;
    font-size: 20px;
  }
</style>
