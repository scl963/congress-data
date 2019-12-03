<template>
  <VoteContainer :voteData="voteData" />
</template>

<script>
import VoteContainer from "~/components/VoteContainer";
import axios from "axios";

export default {
  components: {
    VoteContainer
  },
  async asyncData({ query, route }) {
    const chamber = route.path.split("/").filter(el => el != "")[0];
    const uri = `https://api.propublica.org/congress/v1/${query.congress}/${chamber}/sessions/${query.sessions}/votes/${query.votes}.json`;
    let { data } = await axios.get(uri, {
      headers: {
        "X-API-Key": process.env.PROPUBLICA_API_KEY
      }
    });
    return { voteData: data.results.votes.vote };
  }
};
</script>
