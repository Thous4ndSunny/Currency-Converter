<template>
  <div id="app">
    <app-countryBox v-if="countryOn"></app-countryBox>

    <app-currConvert v-if="converterOn"></app-currConvert>
    <button @click="goToConvert()" class="showerBtn" v-if="countryBtn">
      Convert some currencies
    </button>
    <button @click="goToConvert()" class="showerBtn" v-if="!countryBtn">
      Show the country
    </button>
  </div>
</template>

<script>
import countryBox from "./countryBox.vue";
import currConvert from "./currConvert.vue";

export default {
  components: {
    "app-countryBox": countryBox,
    "app-currConvert": currConvert
  },

  data() {
    return {
      countryBtn: true,
      show: false,
      converterOn: false,
      countryOn: true
    };
  },
  methods: {
    clickToShow: function() {
      this.show = !this.show;
    },
    goToConvert: function() {
      this.countryBtn = !this.countryBtn;
      this.converterOn = !this.converterOn;
      this.countryOn = !this.countryOn;
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  font-family: "Nunito", sans-serif;
}
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  height: 100vh;
  background-color: #222;
}

.showerBtn {
  /* position: absolute; */
  top: 400px;
  margin: 0;
  background: #5a5a6b;
  height: 60px;
  width: 200px;
  border: none;
  border-radius: 30px;
  background-color: #32a88f;
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 10px;
}
</style>
