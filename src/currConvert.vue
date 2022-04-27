<template>
  <div id="currConvert">
    <div class="convert-container" v-if="rendered">
      <span class="con1">
        <!-- <img v-bind:src="firstFlag" /> -->
        <input type="number" v-model="amount" />
      </span>
      <span class="sel1"
        ><select name="conv1" v-model="pickedCurrFrom" @change="showMeConv">
          <option v-for="curr in conversionRatesArray" :key="curr">{{
            curr
          }}</option></select
        ></span
      >
      <span class="con2">
        <!-- <img /> -->
        <div class="output">
          <p>{{ calculation1 }}</p>
        </div>
      </span>
      <span class="sel2"
        ><select name="to" id="" v-model="pickedCurrTo" @change="convertMoney">
          <option v-for="ar in conversionRatesArray" :key="ar">{{ ar }}</option>
        </select></span
      >
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rendered: false,
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
      amount: 1,
      allDataNeeded: null
      // firstFlag: null
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
        this.rendered = true;
      });
  },
  // mounted() {
  //   fetch("https://restcountries.com/v2/all")
  //     .then(response => response.json())
  //     .then(data => (this.allDataNeeded = data));
  // },

  computed: {
    convertMoney: function() {
      this.calculation1 =
        Number(this.conversionRateObject[this.pickedCurrTo]) *
        Number(this.amount);
    },
    showMeConv: function() {
      fetch(
        `https://v6.exchangerate-api.com/v6/a338f9df71740d0a64e08a2d/latest/${this.pickedCurrFrom}`
      )
        .then(response => response.json())
        .then(data => {
          this.conversionRateObject = data.conversion_rates;
          this.conversionRatesArray = Object.keys(data.conversion_rates);
          // if (this.pickedCurrFrom === "EUR") {
          //   this.allDataNeeded.forEach(el => {
          //     if (el.name === "European Union") {
          //       this.firstFlag = el.flags.svg;
          //     }
          //   });
          // } else {
          //   this.allDataNeeded.forEach(el => {
          //     if (this.pickedCurrFrom === el.currencies[0].code) {
          //       this.firstFlag = el.flags.svg;
          //     }
          //   });
          // }
        });
    }
  }
};
</script>

<style scoped>
#currConvert {
  /* margin-top: 30px; */
  margin: 200px;
}
.convert-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: auto;
  width: 300px;
  border: 3px none;
  border-radius: 5px;
  height: 300px;
  align-items: center;
  border: 3px solid #32a88f;
  background-color: #222;
}

span {
  border: none;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;

  background-color: #2d2d37;
}

.sel1,
.sel2 {
  width: 70px;
  height: 80px;
  margin-right: 10px;
  border: 2px solid #32a88f;
}

.con1,
.con2 {
  width: 200px;
  height: 80px;
  margin-right: 10px;
  margin-left: 10px;
  border: 2px solid #32a88f;
}

img {
  width: 60px;
  height: 60px;
  background-color: #222;
}

input {
  width: 120px;
  height: 35px;
  margin-left: 5px;
  border: none;
  text-align: center;
  font-weight: 600;
  font-size: 18px;
  color: #32a88f;

  border-radius: 5px;
  border: 1px solid #32a88f;
  background-color: #222;
  -webkit-appearance: none;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

.output {
  width: 120px;
  height: 35px;
  margin-left: 5px;
  text-align: center;
  font-weight: 600;
  font-size: 18px;
  color: #32a88f;
  border: 1px solid #32a88f;
  border-radius: 5px;
  background-color: #222;
  display: flex;
  align-items: center;
  justify-content: center;
}

select {
  width: 50px;
  height: 35px;
  border-radius: 5px;
  background-color: #222;
  text-align: center;
  text-align: center;
  font-weight: 600;
  font-size: 12px;
  color: #32a88f;
  border: 1px solid #32a88f;
  color: #32a88f;
  -webkit-appearance: none;
}

p {
  text-align: center;
  font-weight: 600;
  font-size: 18px;
  color: #32a88f;
}
</style>
