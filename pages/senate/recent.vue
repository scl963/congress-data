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
      "https://api.propublica.org/congress/v1/senate/votes/recent.json",
      {
        headers: {
          "X-API-Key": process.env.PROPUBLICA_API_KEY
        }
      }
    );
    return { voteData: data.results };
  }
};
</script>
