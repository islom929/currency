<script setup>
import { onUpdated, reactive, ref, watchEffect } from 'vue'

  const currencies = reactive([
    'usd','btc','eur','eth'
  ])
  const currencyFrom = ref('btc')
  const currencyTo = ref('eur')
  const currencyHave = reactive({
    value: '',
    disabled: false
  })
  const currencyNeed = reactive({
    value: '',
    disabled: false
  })
  const currencyRate = ref('')
  const currencyRateUsd = ref('')

  watchEffect(() => {
    const getData = async () => {
      let formData = new FormData()
      formData.append('currency_from', currencyFrom.value)
      formData.append('currency_to', currencyTo.value)
      let res = await fetch('https://api.crypto-verse.info/api/calculator/exchange/calculate',{
        method: 'POST',
        headers: {
          "Accept":"application/json",
        },
        body: formData
      })
      let data = await res.json()
        currencyRate.value = JSON.parse(data?.data?.conversion_rate)
        currencyRateUsd.value = JSON.parse(data?.data?.conversion_rate_usd)
      }

    getData()
  })

  onUpdated(() => {
    if (currencyHave.disabled) {
      currencyHave.value = (currencyNeed.value * currencyRate.value).toFixed(3)
      if (currencyFrom.value == 'usd') {
        currencyRateUsd.value = currencyRate.value
      }
    }
    if (currencyNeed.disabled) {
      currencyNeed.value = (currencyHave.value * currencyRate.value).toFixed(3)
    }
  })


  const checkingInputFunc = (evt) => {
    if(evt.target.classList.contains('input--have')) {
      currencyNeed.disabled = true
    }
    if(evt.target.classList.contains('input--need')) {
      currencyHave.disabled = true
    }
  }

  const inputBlurFunc = () => {
    if(!currencyHave.value) {
      currencyNeed.disabled = false
    }
    if(!currencyNeed.value) {
      currencyHave.disabled = false
    }
    if (currencyHave.value === '' || currencyNeed.value === '') {
      currencyHave.value = ''
      currencyNeed.value = ''
    }
  }

</script>

<template>
  <div class="card">
      <form method="post">
      <div class="flex items-center justify-between">
        <h1 class="text-lg">Amount I have</h1>
          <span class="text-base text-gray-600">{{`${currencyHave.value} ${currencyFrom}`}}</span>
        </div>
        <div class="flex items-center justify-between">
          <select name="from" v-model="currencyFrom" @change="() => {currencyHave.value = '', currencyNeed.value = ''}">
            <option v-for="item of currencies" :key="item" v-show="currencyTo !== item" :value="item">{{item}}</option>
          </select>
          <input :disabled="currencyHave.disabled" class="input--have " @input="checkingInputFunc" @blur="inputBlurFunc" v-model="currencyHave.value" type="number">
        </div>
        <div>
          <h2 class="text-lg">I need</h2>
        <div class="flex items-center justify-between">
          <select name="from" v-model="currencyTo" @change="() => {currencyHave.value = '', currencyNeed.value = ''}">
             <option v-for="item of currencies" :key="item" v-show="currencyFrom !== item" :value="item">{{item}}</option>
          </select>
          <input :disabled="currencyNeed.disabled" class="input--need" @input="checkingInputFunc" @blur="inputBlurFunc" v-model="currencyNeed.value" type="number">
        </div>
      </div>
      </form>
    <div class="result-usd" v-show="currencyHave.disabled">Rate: ${{ currencyNeed.value ? (currencyNeed.value * currencyRateUsd).toFixed(3) : currencyRateUsd }}</div>
    <div class="result-usd" v-show="currencyNeed.disabled">Rate: ${{ currencyHave.value ? (currencyHave.value * currencyRateUsd).toFixed(3) : currencyRateUsd }}</div>
  </div>
</template>

<style lang="css" scoped>

  .card {
    width: 75%;
    height: 400px;
    margin: 0 30px 0 auto;
    padding: 25px;
    border-top: 2px solid rgb(0, 123, 255);
    background-color: #fff;
  }

  input {
    width: auto;
    padding: 3px 10px;
    border: 1px solid #999;
    border-radius: 3px;
    background-color: rgb(242, 242, 242);
    color: #000;
    text-align: center;
    font-size: 20px;
  }

  .result-usd {
    width: 90%;
    margin: 20px auto 0 auto;
    padding: 10px 20px;
    text-align: center;
    background-color: rgb(115, 179, 238);
    color: #fff;
    border-radius: 10px;
  }

</style>