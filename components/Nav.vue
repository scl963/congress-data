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
  created() {
    this.currentPage = this.links.find(
      link => link.to === this.$router.currentRoute.path
    ).text;
  },
  mounted() {
    this.currentPage = this.links.find(
      link => link.to === this.$router.currentRoute.path
    ).text;

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
  },
  methods: {
    setCurrentPage(pageName) {
      console.log(pageName);
      this.currentPage = pageName;
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
    background: white;
    transition: all 0.2s ease;
  }
}

.nav-link {
  color: black;
  display: inline-block;
  font-size: 20px;
  text-decoration: none;
  z-index: 2;

  &.selected {
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
