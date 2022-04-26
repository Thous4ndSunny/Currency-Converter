<template>
  <div id="currConvert">
    <div class="convert-container">
      <span class="1"><input type="number" v-model="amount"/></span>
      <span class="2">
        <select name="conv1" v-model="pickedCurrFrom" @change="showMeConv">
          <option v-for="curr in conversionRatesArray" :key="curr">{{
            curr
          }}</option>
        </select>
      </span>
      <span class="3">{{ calculation1 }}</span>
      <span class="4">
        <select name="to" id="" v-model="pickedCurrTo" @change="convertMoney">
          <option v-for="ar in conversionRatesArray" :key="ar">{{ ar }}</option>
        </select>
      </span>
    </div>
    <button @click="goToConvert()" class="showerBtn">
      Convert some currencies
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      mislav: false,
      ali: false,
      show: false,
      thisis: "This is the currency converter",
      showConvert: false,
      conversionRateObject: null,
      conversionRatesArray: null,
      pickedCurrFrom: "EUR",
      pickedCurrTo: "USD",
      calculation: null,
      calculation1: null,
      amount: 1
    };
  },
  methods: {},
  created() {
    fetch(
      `https://v6.exchangerate-api.com/v6/a338f9df71740d0a64e08a2d/latest/EUR`
    )
      .then(response => response.json())
      .then(data => {
        this.conversionRateObject = data.conversion_rates;
        this.conversionRatesArray = Object.keys(data.conversion_rates);
      });
  },
  computed: {
    convertMoney: function() {
      this.calculation1 = `${Number(
        this.conversionRateObject[this.pickedCurrTo]
      ) * Number(this.amount)} ${this.pickedCurrTo}`;
    },
    showMeConv: function() {
      fetch(
        `https://v6.exchangerate-api.com/v6/a338f9df71740d0a64e08a2d/latest/${this.pickedCurrFrom}`
      )
        .then(response => response.json())
        .then(data => {
          this.conversionRateObject = data.conversion_rates;
          this.conversionRatesArray = Object.keys(data.conversion_rates);
        });
    }
  }
};
</script>

<style scoped>
.convert-container {
  max-width: 600px;
  height: 300px;
  margin: 0 auto;
  border: 4px solid;
  border-radius: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);

  /* row-gap: 1px;
  column-gap: 1px; */
}

span {
  width: 250px;
  height: 100px;
  border: 2px solid;
  border-radius: 20px;
  /* margin-top: 5px; */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  row-gap: 1px;
  column-gap: 1px;
}
</style>
