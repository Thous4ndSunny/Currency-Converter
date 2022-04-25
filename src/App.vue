<template>
  <div>
    <app-countryBox></app-countryBox>
  </div>
</template>

<script>
import countryBox from "./countryBox.vue";

export default {
  components: {
    "app-countryBox": countryBox
  },

  data() {
    return { show: false };
  },
  methods: {
    clickToShow: function() {
      this.show = !this.show;
    }
  }
};
</script>

<style>
/* we will explain what these classes do next! */
</style>
