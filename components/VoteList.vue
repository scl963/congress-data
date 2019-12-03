<template>
  <div class="list-container">
    <h1 class="list-title">Recent {{ chamber }} Votes</h1>
    <div class="vote-list">
      <nuxt-link
        v-for="(vote, idx) in votes"
        :key="idx"
        class="vote-card"
        :to="`vote/?${vote.queryString}`"
      >
        <h4>{{ vote.bill.title || vote.description }}</h4>
      </nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    voteData: {
      type: Object,
      default: () => {}
    }
  },
  computed: {
    chamber() {
      return this.voteData.chamber;
    },
    votes() {
      return this.voteData.votes.map(vote => {
        vote.queryString = this.generateQuery(vote.vote_uri);
        return vote;
      });
    }
  },
  methods: {
    generateQuery(str) {
      const params = str.match(/(?<=v1\/)(.*)(?=.json)/)[0];
      const paramArr = params.split("/");
      console.log(paramArr);
      return `congress=${paramArr[0]}&sessions=${paramArr[3]}&votes=${paramArr[5]}`;
    }
  }
};
</script>

<style lang="scss" scoped>
.list-container {
  margin: 2%;
}

.list-title {
  margin: 30px auto;
  text-align: center;
  position: sticky;
  top: 5px;
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
</style>
