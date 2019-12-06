<template>
  <div class="list-container">
    <h1 class="list-title">Recent {{ chamber }} Votes</h1>
    <Table v-if="table" :votes="votes" />
    <div v-else class="vote-list">
      <nuxt-link
        v-for="(vote, idx) in votes"
        :key="idx"
        class="vote-card"
        :to="`vote/?${vote.queryString}`"
      >
        <h4>{{ vote.bill.title || vote.description }}</h4>
      </nuxt-link>
    </div>
    <button class="load-more-button" @click="loadMore">Load more...</button>
  </div>
</template>

<script>
import Table from "./Table";

export default {
  components: {
    Table
  },
  props: {
    voteData: {
      type: Object,
      default: () => {}
    },
    loadMore: {
      type: Function,
      default: () => {}
    }
  },
  data() {
    return {
      table: true
    };
  },
  computed: {
    chamber() {
      return this.voteData.chamber;
    },
    votes() {
      return this.voteData.votes.map(vote => {
        const title = vote.bill.title || vote.description;
        vote.queryString = this.generateQuery(vote.vote_uri);
        vote.shortTitle = this.generateShortTitle(title);
        return vote;
      });
    }
  },
  methods: {
    generateQuery(str) {
      const params = str.match(/v1\/(.*).json/)[1];
      const paramArr = params.split("/");
      return `congress=${paramArr[0]}&sessions=${paramArr[3]}&votes=${paramArr[5]}`;
    },
    generateShortTitle(title) {
      const ellipsis = title.length > 150 ? "..." : "";
      return title.slice(0, 150) + ellipsis;
    }
  }
};
</script>

<style lang="scss" scoped>
.list-container {
  align-items: center;
  display: flex;
  flex-direction: column;
  margin: 70px 2%;
}

.list-title {
  font-size: 1.5rem;
  margin: 30px auto;
  max-width: 340px;
  text-align: center;
  position: sticky;
  top: 23px;
  z-index: 1;
}

.vote-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  grid-auto-rows: 250px;
  grid-gap: 20px;
  max-width: 1000px;
  margin: 40px auto;
}

.vote-card {
  background: white;
  border-radius: 3px;
  box-shadow: 6px 5px 88px -61px rgba(0, 0, 0, 0.75);
  cursor: pointer;
  margin: 10px;
  padding: 10px;

  &:hover {
    background: #e4e4e4;
  }
}

.load-more-button {
  border: none;
  background: #404040;
  color: #ffffff;
  cursor: pointer;
  margin-top: 40px;
  padding: 20px;
  text-transform: uppercase;
  border-radius: 6px;
  display: inline-block;
  transition: all 0.3s ease 0s;

  &:hover {
    color: #404040;
    font-weight: 700;
    letter-spacing: 3px;
    background: none;
    box-shadow: 0px 5px 40px -10px rgba(0, 0, 0, 0.57);
    transition: all 0.3s ease 0s;
  }
}
</style>
