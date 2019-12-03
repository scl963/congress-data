<template>
  <VoteList :voteData="voteData" />
</template>

<script>
import VoteList from "~/components/VoteList";
import axios from "axios";

export default {
  components: {
    VoteList
  },
  async asyncData() {
    let { data } = await axios.get(
      "https://api.propublica.org/congress/v1/house/votes/recent.json",
      {
        headers: {
          "X-API-Key": "yiaMPHgFwJxJLjUGvFtCTx1Arnm31Q1lA0SBRjdK"
        }
      }
    );
    return { voteData: data.results };
  }
};
</script>
