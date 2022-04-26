<template>
  <transition>
    <div id="countryBox" v-if="readyToRoll">
      <div class="country-card">
        <img v-bind:src="countryFlag" alt="" />
        <div class="country-info-data">
          <h1>{{ countryName }}</h1>
          <p>{{ countryRegion }}</p>
          <p>{{ countryCapital }}</p>
          <p>{{ countryLanguage }}</p>
          <p>{{ countryPopulation }}</p>
          <p>{{ countryCurrCode }}</p>
          <p>{{ countryCurrName }}</p>
          <select
            name="Change"
            @change="renderSelectedCountry()"
            id=""
            v-model="pickedCountry"
          >
            <option v-for="option in listOfCountryNames" :key="option">{{
              option
            }}</option>
          </select>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  data() {
    return {
      readyToRoll: false,
      currentLocationCountry: null,
      allCountriesData: null,
      allData: null,
      // Country infomration
      countryName: null,
      countryRegion: null,
      countryCapital: null,
      countryCurrName: null,
      countryCurrCode: null,
      countryFlag: null,
      countryPopulation: null,
      countryLanguage: null,
      listOfCountryNames: null,
      pickedCountry: null
    };
  },
  methods: {
    renderSelectedCountry: function() {
      this.allData.forEach(el => {
        if (el.name === this.pickedCountry) {
          this.countryName = el.name;

          this.countryRegion = el.region;
          this.countryCapital = el.capital;
          this.countryFlag = el.flags.svg;
          this.countryPopulation = this.calcPopulation(el.population);

          this.countryCurrName = el.currencies[0].code;

          this.countryCurrCode = el.currencies[0].name;
          this.countryLanguage = el.languages[0].name;
          this.pickedCountry = el.name;
        }
      });
    },

    getListOfCountries: function(allCountriesData) {
      const countryList = allCountriesData.map(el => el.name);
      this.listOfCountryNames = countryList;
      console.log(countryList);
    },

    getCountryInfo: function(dataObject, countryCode = 0, countryName = 0) {
      dataObject.forEach(el => {
        if (el.alpha2Code === countryCode || el.name === countryName) {
          this.countryName = el.name;

          this.countryRegion = el.region;
          this.countryCapital = el.capital;
          this.countryFlag = el.flags.svg;
          this.countryPopulation = this.calcPopulation(el.population);

          this.countryCurrName = el.currencies[0].code;

          this.countryCurrCode = el.currencies[0].name;
          this.countryLanguage = el.languages[0].name;
          this.pickedCountry = el.name;
        }
      });
    },
    calcPopulation: function(populationValue) {
      const value = Number(populationValue);
      if (value < 1000000) {
        return (value / 1000).toFixed(1) + " Thousand";
      } else if (value < 1000000000) {
        return (value / 1000000).toFixed(2) + " Milion";
      } else if (value >= 1000000000) {
        return (value / 1000000000).toFixed(3) + " Bilion";
      }
    }
  },
  created() {
    // napravi axisios
    fetch("https://api.country.is")
      .then(response => response.json())
      .then(countryLocIP => {
        //getting the loaction country based on the IP of the user
        const countrycca2Code = countryLocIP.country;
        this.currentLocationCountry = countrycca2Code;
        fetch(`https://restcountries.com/v2/all`)
          .then(response => response.json())
          .then(allCountriesData => {
            //   console.log(allCountriesData);
            //getting the data about all the countries in the world
            this.allData = allCountriesData;
            this.getCountryInfo(allCountriesData, countrycca2Code);
            this.getListOfCountries(allCountriesData);
            this.readyToRoll = true;
          });
      });
  },
  mounted() {
    // this.readyToRoll = true;
  }
};
</script>

<style scoped>
* {
  margin: 0;
}
.country-card {
  margin: 0 auto;
  max-width: 300px;
  height: 600px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  border-radius: 20px;
  border: 4px solid;
}
.country-info-data {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  border-radius: 20px;
  width: 100%;
  height: 100%;
}

img {
  width: 100%;
  max-height: 150px;
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
  border-bottom: 4px solid;
}

select {
  width: 100%;
  height: 3rem;
  padding: 0.5px;
  border: none;
  text-align: center;
  color: blue;

  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
  background: lightgrey;
}
</style>
