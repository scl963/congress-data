<template>
  <div class="container">
    <h1 class="vote-title">
      {{ voteData.bill.title || voteData.description }}
    </h1>
    <VoteVisualization :vote="voteData" />
    <PartisanshipMeter :vote="voteData" />
  </div>
</template>

<script>
import PartisanshipMeter from "./PartisanshipMeter";
import VoteVisualization from "./VoteVisualizaton";

export default {
  components: {
    PartisanshipMeter,
    VoteVisualization
  },
  props: {
    voteData: {
      default: () => {},
      type: Object
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  align-items: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 70px auto;
  min-height: calc(100vh - 140px);
  text-align: center;
}

.vote-title {
  font-size: 1.3rem;
  max-width: 800px;
  width: 100%;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.tooltip-inner {
  background: black;
  border-radius: 16px;
  color: white;
  font-weight: bold;
  padding: 5px 10px 4px;
}

.links {
  padding-top: 15px;
}
</style>
