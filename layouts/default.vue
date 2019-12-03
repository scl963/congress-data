<template>
  <div>
    <Nav />
    <nuxt />
  </div>
</template>

<script>
import Vue from "vue";
import VTooltip from "v-tooltip";
import Nav from "~/components/Nav";

Vue.use(VTooltip);

export default {
  components: {
    Nav
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Work+Sans&display=swap");

html {
  font-family: "Work Sans", sans-serif, -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}
</style>
