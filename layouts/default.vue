<template>
  <div>
    <Nav />
    <nuxt id="content" />
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
  margin: 0;
}

body {
  margin: 0;
}

#content {
  margin: 70px 2%;
}
</style>
