<template>
  <div id="app">
    <app-countryBox></app-countryBox>

    <app-currConvert></app-currConvert>
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
      show: false
    };
  },
  methods: {
    clickToShow: function() {
      this.show = !this.show;
    },
    goToConvert: function() {
      this.showCountry = !this.showCountry;
      this.showConvert = !this.showConvert;
    }
  }
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 100vh;
}

.showerBtn {
  position: absolute;
  top: 400px;
  margin: 300px auto;
}
</style>
