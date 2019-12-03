<template>
  <nav id="navbar">
    <nuxt-link class="nav-link" to="/house/recent">House</nuxt-link>
    <nuxt-link class="nav-link" to="/senate/recent">Senate</nuxt-link>
  </nav>
</template>

<script>
export default {
  mounted() {
    window.onscroll = function() {
      scrollFunction();
    };

    function scrollFunction() {
      if (
        document.body.scrollTop > 40 ||
        document.documentElement.scrollTop > 40
      ) {
        document.getElementById("navbar").setAttribute("class", "collapsed");
      } else {
        document.getElementById("navbar").setAttribute("class", "expanded");
      }
    }
  }
};
</script>

<style lang="scss" scoped>
#navbar {
  height: 70px;
  position: fixed;
  top: 0;
  width: 100%;

  &.collapsed {
    box-shadow: 0 0 5px 0 rgba(0, 0, 0, 0.75);
    background: white;
    transition: all 0.2s ease;
  }
}

.nav-link {
  color: black;
  display: inline-block;
  font-size: 20px;
  margin: 24px 15px;
  text-decoration: none;
  z-index: 2;

  &:hover {
    color: blue;
    transform: scale(1.1);
    transition: all 0.2s ease;
  }
}
</style>
