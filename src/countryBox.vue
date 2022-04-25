<template>
  <transition>
    <div id="countryBox" v-if="readyToRoll">
      <div class="country-card">
        <img v-bind:src="countryFlag" alt="" />
        <div class="country-info-data">
          <h2>{{ countryName }}</h2>
          <h2>{{ countryRegion }}</h2>
          <h2>{{ countryCapital }}</h2>
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
      // Country infomration
      countryName: null,
      countryRegion: null,
      countryCapital: null,
      // countryCurrName = null,
      // countryCurrCode = null,
      countryFlag: null
    };
  },
  methods: {
    getCountryInfo: function(dataObject, countryCode) {
      console.log("im running");
      console.log(dataObject);
      dataObject.forEach(el => {
        console.log("also");
        if (el.alpha2Code === countryCode) {
          this.countryName = el.name;
          console.log(this.countryName);
          this.countryRegion = el.region;
          this.countryCapital = el.capital;
          this.countryFlag = el.flags.svg;
          //   this.countryCurrName = el.
          //   this.countryCurrCode = el.
        }
      });
    }
  },
  created() {
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

            this.getCountryInfo(allCountriesData, countrycca2Code);
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
.country-card {
  margin: 0 auto;
  max-width: 300px;
  height: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  border: 2px solid;
}
.country-info-data {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 20px;
}

img {
  width: 100%;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
}

/* TRansition */
.v-enter-active,
.v-leave-active {
  transition: opacity 5s ease;
}

.v-enter-from,
.v-leave-to {
  scale: 0.2;
  opacity: 0;
}
</style>
