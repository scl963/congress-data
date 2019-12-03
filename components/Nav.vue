<template>
  <nav id="navbar">
    <nuxt-link class="nav-link" to="/house/recent">House</nuxt-link>
    <nuxt-link class="nav-link" to="/senate/recent">Senate</nuxt-link>
  </nav>
</template>

<style lang="scss" scoped>
#navbar {
  align-items: center;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  height: 40px;
  position: fixed;
  top: 0;
}

.nav-link {
  color: black;
  font-size: 20px;
  text-decoration: none;
  margin: 10px 15px;

  &:hover {
    color: blue;
    transform: scale(1.1);
    transition: all 0.2s ease;
  }
}
</style>
