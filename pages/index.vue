<template>
  <div class="container">
    <Votes :data="voteData"/>
  </div>
</template>

<script>
import Votes from '~/components/Votes';
import axios from 'axios';

export default {
  components: {
    Votes
  },
  data () {
    return {
    }
  },
  async asyncData () {
    let { data } = await axios.get('https://api.propublica.org/congress/v1/house/votes/recent.json', {
      headers: {
        "X-API-Key": "yiaMPHgFwJxJLjUGvFtCTx1Arnm31Q1lA0SBRjdK"
      }
    })
    return { voteData: data.results };
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
