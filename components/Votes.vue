<template>
  <div ref="d3-chart" >
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  props: ['data'],
  data () {
    return {
      selectedVote: 2,
      width: 1000,
      height: 500,
      numYes: 0,
      numNo: 0,
      numAbstain: 0
    }
  },
  computed: {
    selectedVoteTotals () {
      return this.data.votes[this.selectedVote];
    },
    maxRows () {
      return Math.max(...Object.values(this.selectedVoteTotals.total)) / 15
    },
    circleRadius () {
      return Math.min((this.width / 100), (this.height / this.maxRows));
    },
    circleOffset () {
      return this.circleRadius;
    },
    yesVote () {
      const startX = this.circleRadius;
      const startY = this.height;
      const x = startX + (this.numYes % 15) * (this.circleRadius + this.circleOffset);
      const y = startY - Math.ceil((this.numYes + 1) / 15) * (this.circleRadius + this.circleOffset);
      return { x, y };
    },
    noVote () {
      const startX = this.width - this.circleRadius;
      const startY = this.height;
      const x = startX - (this.numNo % 15) * (this.circleRadius + this.circleOffset);
      const y = startY - Math.ceil((this.numNo + 1) / 15) * (this.circleRadius + this.circleOffset);
      return { x, y };
    }
  },
  methods: {
    plotRepublicanVotes (context) {
      const { yes, no, not_voting } = this.selectedVoteTotals.republican;
      this.plotVotes(context, yes, 'yesVote', 'red');
      this.plotVotes(context, no, 'noVote', 'red');
    },
    plotDemocratVotes (context) {
      const { yes, no, not_voting } = this.selectedVoteTotals.democratic;
      this.plotVotes(context, yes, 'yesVote', 'blue');
      this.plotVotes(context, no, 'noVote', 'blue');
    },
    plotIndependentVotes (context) {
      const { yes, no, not_voting } = this.selectedVoteTotals.independent;
      this.plotVotes(context, yes, 'yesVote', 'gray');
      this.plotVotes(context, no, 'noVote', 'gray');
    },
    plotVotes (context, numVotes, voteType, color) {
      let updateVariable = 'numAbstain';
      if (voteType === 'yesVote') {
        updateVariable = 'numYes';
      } else if (voteType === 'noVote') {
        updateVariable = 'numNo';
      }
      console.log(updateVariable)
      for (let i = 0; i < numVotes; i++) {
        const { x, y } = this[voteType];
        context.append("circle")
        .attr("cx", x)
        .attr("cy", y)
        .attr("r", this.circleRadius)
        .style("fill", color);
        this[updateVariable] += 1;
      }
    },
  },
  // Loop through each party's votes, have an x and y position saved for both yes and no, color circle based on party
  mounted () {
    const chart = d3.select(this.$refs["d3-chart"])
    .append("svg")
    .attr("width", this.width)
    .attr("height", this.height);
    this.plotRepublicanVotes(chart);
    this.plotDemocratVotes(chart);
    this.plotIndependentVotes(chart);
  }
}
</script>

<style lang="scss" scoped>
.votes-container {
  font-size: 20px;
  font-family: Arial, Helvetica, sans-serif;
}
</style>