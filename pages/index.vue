<template>
  <div class="landing-page-container">
    <NewsFeed :articles="articles" title="Congressional News" />
  </div>
</template>

<script>
import NewsFeed from "~/components/NewsFeed";
import axios from "axios";

export default {
  components: {
    NewsFeed
  },
  async asyncData() {
    let { data } = await axios.get(
      "https://newsapi.org/v2/everything?q=congressional%20vote&sortby=publishedAt&language=en",
      {
        headers: {
          "X-Api-Key": process.env.NEWS_API_KEY
        }
      }
    );
    return { articles: data.articles };
  }
};
</script>

<style lang="scss" scoped>
.landing-page-container {
  display: grid;
  grid-template-columns: 100%;
  grid-template-rows: 100%;
  grid-gap: 40px;
  height: calc(100vh - 140px);
  overflow: hidden;
}

@media only screen and (min-width: 900px) {
  .landing-page-container {
    flex-direction: row;
    grid-template-columns: 1fr 1fr;
  }
}
</style>
