<template>
  <transition>
    <div id="countryBox" v-if="readyToRoll">
      <div class="country-card">
        <div class="img-cont">
          <span><img v-bind:src="countryFlag" alt=""/></span>
        </div>

        <span class="span-country"
          ><h1>{{ countryName }}</h1></span
        >
        <span class="span-text"
          ><span class="desc-cont">
            <p class="desc">Region</p>
          </span>
          <p class="out">{{ countryRegion }}</p></span
        >
        <span class="span-text"
          ><span class="desc-cont"><p class="desc">Capital</p></span>
          <p class="out">{{ countryCapital }}</p></span
        >
        <span class="span-text"
          ><span class="desc-cont">
            <p class="desc">Language</p>
          </span>
          <p class="out">{{ countryLanguage }}</p></span
        >
        <span class="span-text"
          ><span class="desc-cont">
            <p class="desc">Population</p>
          </span>
          <p class="out">{{ countryPopulation }}</p></span
        >
        <span class="span-text"
          ><span class="desc-cont">
            <p class="desc">Currency</p>
          </span>
          <p class="out">{{ countryCurrName }}</p></span
        >

        <span class="span-select"
          ><select
            name="Change"
            @change="renderSelectedCountry()"
            id=""
            v-model="pickedCountry"
          >
            <option v-for="option in listOfCountryNames" :key="option">{{
              option
            }}</option>
          </select></span
        >
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
      // console.log(countryList);
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
    // napravi axios
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
#countryBox {
  margin: 20px;
}
.country-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  border: 5px solid #32a88f;
  width: 300px;
  height: 650px;
  border-radius: 5px;
  background-color: #222;
  color: #32a88f;
}
img {
  border: none;
  width: 200px;
  height: 150px;
  background-color: #2d2d37;
}

.span-country {
  width: 290px;
  height: 90px;
  border: 2px solid #32a88f;
  margin-bottom: 10px;
  border-radius: 5px;
  background-color: #2d2d37;
  color: #32a88f;
  text-align: center;
}
h1 {
  font-size: 24px;
  margin: 15px auto;
}
.span-text {
  width: 250px;
  height: 60px;
  border: 2px solid #32a88f;
  margin-bottom: 5px;
  border-radius: 5px;
  background-color: #2d2d37;
  color: #32a88f;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.span-select {
  width: 300px;
  height: 60px;
  margin-bottom: 0;
  border: none;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  background-color: #32a88f;
}

select {
  width: 300px;
  height: 60px;
  margin-bottom: 0;
  border: none;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  background-color: #32a88f;
  text-align: center;
  font-size: 18px;
  /* -webkit-appearance: none; */
}

.out {
  color: #32a88f;
  font-size: 20px;
  margin-bottom: 2px;
  margin-top: 1px;
}
.desc {
  font-size: 12px;
  margin-top: 2px;
  margin-bottom: 5px;
  font-weight: 600;
  text-transform: uppercase;
}

.desc-cont {
  border: 1px solid #32a88f;
  height: 20px;
  width: 100px;
  text-align: center;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  padding: 0;
  color: #222;
  background-color: #32a88f;
}
.out-cont {
  border: 1px solid;
  height: 10px;
  width: 80px;
}
</style>
