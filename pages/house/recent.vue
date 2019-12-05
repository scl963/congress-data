<template>
  <VoteList :voteData="voteData" :loadMore="loadMore" />
</template>

<script>
import VoteList from "~/components/VoteList";
import axios from "axios";

export default {
  components: {
    VoteList
  },
  data() {
    return {
      offset: 1
    };
  },
  async asyncData() {
    let { data } = await axios.get(
      "https://api.propublica.org/congress/v1/house/votes/recent.json",
      {
        headers: {
          "X-API-Key": process.env.PROPUBLICA_API_KEY
        }
      }
    );
    return { voteData: data.results };
  },
  methods: {
    async loadMore() {
      let { data } = await axios.get(
        `https://api.propublica.org/congress/v1/house/votes/recent.json?offset=${this.offset}`,
        {
          headers: {
            "X-API-Key": process.env.PROPUBLICA_API_KEY
          }
        }
      );
      console.log(data);
      console.log(this.voteData.votes);
      this.voteData.votes = [...this.voteData.votes, ...data.results.votes];
    }
  }
};
</script>
