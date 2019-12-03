<template>
  <div class="partisanship-container">
    <div class="meter-title">
      Party Unity<img
        v-tooltip="
          'Calculated by averaging the proportion of each party that voted with the party majority'
        "
        class="info-tooltip"
        src="~/assets/information.svg"
      />
    </div>
    <div class="meter-background">
      <div
        class="meter-progress"
        :style="{ width: `${partisanshipPercentage}%` }"
      />
    </div>
    <div class="meter-legend">
      {{ Math.round(partisanshipPercentage) + "%" }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    vote: {
      default: () => {},
      type: Object
    }
  },
  computed: {
    partisanshipPercentage() {
      const democraticDiscipline = this.calcPartyDiscipline(
        this.vote.democratic
      );
      const republicanDiscipline = this.calcPartyDiscipline(
        this.vote.republican
      );
      return (democraticDiscipline + republicanDiscipline) / 2;
    }
  },
  methods: {
    calcPartyDiscipline({ no, present, yes }) {
      const totalVotes = no + present + yes;
      const maxVal = Math.max(no, present, yes);
      return (maxVal / totalVotes) * 100;
    }
  }
};
</script>

<style lang="scss" scoped>
.partisanship-container {
  max-width: 900px;
  padding: 40px;
  width: 100%;
}

.meter-legend,
.meter-title {
  font-size: 2rem;
}

.info-tooltip {
  cursor: pointer;
  height: 20px;
  margin-left: 5px;
  width: 20px;
}

.meter-background {
  padding: 6px;
  border-radius: 30px;
  background: #e3e3e3;
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.25),
    0 1px rgba(255, 255, 255, 0.08);

  .meter-progress {
    height: 40px;
    border-radius: 30px;
    background: linear-gradient(
      90deg,
      rgba(149, 0, 17, 0.24833683473389356) 13%,
      rgba(220, 0, 0, 1) 100%
    );
    min-width: 5%;
    transition: 0.4s linear;
    transition-property: width, background-color;
  }
}
</style>
