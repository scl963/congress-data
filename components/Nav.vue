<template>
  <nav id="navbar">
    <nuxt-link
      v-for="(link, idx) in links"
      :key="idx"
      class="nav-link"
      :to="link.to"
      >{{ link.text }}</nuxt-link
    >
  </nav>
</template>

<script>
export default {
  data() {
    return {
      links: [
        {
          to: "/",
          text: "Home"
        },
        {
          to: "/house/recent",
          text: "House"
        },
        {
          to: "/senate/recent",
          text: "Senate"
        }
      ],
      currentPage: "Home"
    };
  },
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
  background: white;
  height: 70px;
  position: fixed;
  top: 0;
  width: 100%;

  &.collapsed {
    transition: all 0.2s ease;
  }
}

.nav-link {
  color: black;
  display: inline-block;
  font-size: 20px;
  text-decoration: none;
  z-index: 2;

  &.selected,
  &.nuxt-link-exact-active {
    color: blue;
  }
}

@media only screen and (min-width: 900px) {
  .nav-link {
    margin: 24px 0px 24px 30px;

    &:hover {
      color: blue;
      transform: scale(1.1);
      transition: all 0.2s ease;
    }
  }
}
</style>
