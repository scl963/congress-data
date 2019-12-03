<template>
  <svg
    id="vote-chart"
    ref="vote-chart"
    x="0"
    y="0"
    :alt="altText"
    viewBox="0 0 1000 550"
    xmlns="http://www.w3.org/2000/svg"
  >
    <text
      :x="width * 0.12"
      :y="height + 30"
      font-family="Work sans, sans-serif"
      font-weight="bold"
      font-size="20px"
      fill="black"
    >
      Yes
    </text>
    <text
      :x="width * 0.45"
      :y="height + 30"
      font-family="Work sans, sans-serif"
      font-weight="bold"
      font-size="20px"
      fill="black"
    >
      Abstain
    </text>
    <text
      :x="width * 0.8"
      :y="height + 30"
      font-family="Work sans, sans-serif"
      font-weight="bold"
      font-size="20px"
      fill="black"
    >
      No
    </text>
  </svg>
</template>

<script>
// This component isn't being used anymore, replaced with VoteVisualization.vue

export default {
  props: {
    vote: {
      default: () => {},
      type: Object
    }
  },
  data() {
    return {
      width: 1000,
      height: 500,
      numYes: 0,
      numNo: 0,
      numAbstain: 0
    };
  },
  computed: {
    maxRows() {
      // Calculate the number of rows that will be needed for largest vote total
      return Math.max(...Object.values(this.vote.total)) / 10;
    },
    squareDimensions() {
      // Determine the proper size for each element based on width and height - use smaller of two to ensure fit
      return Math.min(this.width / 70, this.height / this.maxRows);
    },
    offset() {
      // Gap between elements
      return this.squareDimensions / 3;
    },
    // For each vote type, calculate starting location and then keep track of number of votes
    // plotted so far. Maximun row length is 15 elements.
    yesVote() {
      // Yes votes plotted starting from left, with slight margin
      const startX = this.squareDimensions;
      const startY = this.height;
      const x =
        startX + (this.numYes % 15) * (this.squareDimensions + this.offset);
      const y =
        startY -
        Math.ceil((this.numYes + 1) / 15) *
          (this.squareDimensions + this.offset);
      return { x, y };
    },
    abstainVote() {
      // Abstentions plotted starting at 1/3 width of svg container
      const startX = this.squareDimensions + this.width * 0.33;
      const startY = this.height;
      const x =
        startX + (this.numAbstain % 15) * (this.squareDimensions + this.offset);
      const y =
        startY -
        Math.ceil((this.numAbstain + 1) / 15) *
          (this.squareDimensions + this.offset);
      return { x, y };
    },
    noVote() {
      // No votes plotted starting at 2/3 width of svg container
      const startX = this.squareDimensions + this.width * 0.66;
      const startY = this.height;
      const x =
        startX + (this.numNo % 15) * (this.squareDimensions + this.offset);
      const y =
        startY -
        Math.ceil((this.numNo + 1) / 15) *
          (this.squareDimensions + this.offset);
      return { x, y };
    },
    altText() {
      return `For vote: ${this.vote.bill.title}, there were ${this.numYes} yes votes, ${this.numAbstain} abstentions, and ${this.numNo} no votes`;
    }
  },
  mounted() {
    this.makeChart();
  },
  methods: {
    plotRepublicanVotes(chart) {
      const { yes, no, not_voting } = this.vote.republican;
      this.plotVotes(chart, yes, "yesVote", "red");
      this.plotVotes(chart, no, "noVote", "red");
      this.plotVotes(chart, not_voting, "abstainVote", "red");
    },
    plotDemocratVotes(chart) {
      const { yes, no, not_voting } = this.vote.democratic;
      this.plotVotes(chart, yes, "yesVote", "blue");
      this.plotVotes(chart, no, "noVote", "blue");
      this.plotVotes(chart, not_voting, "abstainVote", "blue");
    },
    plotIndependentVotes(chart) {
      const { yes, no, not_voting } = this.vote.independent;
      this.plotVotes(chart, yes, "yesVote", "gray");
      this.plotVotes(chart, no, "noVote", "gray");
      this.plotVotes(chart, not_voting, "abstainVote", "gray");
    },
    plotVotes(chart, numVotes, voteType, color) {
      let updateVariable = "numAbstain";
      if (voteType === "yesVote") {
        updateVariable = "numYes";
      } else if (voteType === "noVote") {
        updateVariable = "numNo";
      }
      for (let i = 0; i < numVotes; i++) {
        const { x, y } = this[voteType];
        const square = document.createElementNS(
          "http://www.w3.org/2000/svg",
          "rect"
        );
        square.setAttribute("x", x);
        square.setAttribute("y", y);
        square.setAttribute("width", this.squareDimensions);
        square.setAttribute("height", this.squareDimensions);
        square.setAttribute("fill", color);
        chart.appendChild(square);
        this[updateVariable] += 1;
      }
    },
    makeChart() {
      const chart = this.$refs["vote-chart"];
      this.plotRepublicanVotes(chart);
      this.plotDemocratVotes(chart);
      this.plotIndependentVotes(chart);
    }
  }
};
</script>

<style lang="scss" scoped>
#vote-chart {
  max-width: 70%;
  max-height: 550px;
}
</style>
