<template>
  <div>
    <h2 class="newsfeed-title">{{ title }}</h2>
    <div class="newsfeed-container">
      <a
        v-for="(article, idx) in articles"
        :key="idx"
        class="news-story"
        :href="article.url"
      >
        <div class="article-info">
          <h3>{{ article.title }}</h3>
          <p>{{ article.description }}</p>
        </div>
        <img class="preview-image" :src="article.urlToImage" />
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    articles: {
      type: Array,
      default: () => {}
    },
    title: {
      type: String,
      default: "Congressional News"
    }
  },
  computed: {
    filteredArticles() {
      return this.articles.filter(article => {
        article.urlToImage.length;
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.newsfeed-container {
  height: calc(100% - 40px);
  overflow-y: scroll;
  width: 100%;
}

.newsfeed-title {
  background: white;
  height: 40px;
  margin: 0;
  text-align: center;
}

.news-story {
  align-items: center;
  border: 1px solid #dadce0;
  border-radius: 8px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 10px;
  padding: 10px;

  & + .news-story {
    margin-top: 10px;
  }

  .article-info {
    max-width: 80%;
  }

  .preview-image {
    border-radius: 8px;
    height: 100px;
    object-fit: cover;
    width: 100px;
  }
}

a {
  color: black;
  text-decoration: none;

  &:hover {
    text-decoration: underline;
  }
}
</style>
