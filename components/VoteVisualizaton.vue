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
    <g id="yes-votes" ref="yes-votes" x="0" y="500"></g>
    <g id="abstain-votes" ref="abstain-votes" x="330" y="500"></g>
    <g id="no-votes" ref="no-votes" x="660" y="500"></g>
    <text
      :x="120"
      :y="540"
      font-family="Work sans, sans-serif"
      font-weight="bold"
      font-size="20px"
      fill="black"
    >
      Yes
    </text>
    <text
      :x="460"
      :y="540"
      font-family="Work sans, sans-serif"
      font-weight="bold"
      font-size="20px"
      fill="black"
    >
      Abstain
    </text>
    <text
      :x="840"
      :y="540"
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
export default {
  props: {
    vote: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      yesVotes: 0,
      noVotes: 0,
      abstentions: 0,
      squareDimensions: 15
    };
  },
  computed: {
    altText() {
      return "Placeholder text";
    },
    yes() {
      const x = (this.yesVotes % 15) * (this.squareDimensions + 2) + 20;
      const y =
        500 - Math.ceil((this.yesVotes + 1) / 15) * (this.squareDimensions + 2);
      return { x, y };
    },
    no() {
      const x = 705 + (this.noVotes % 15) * (this.squareDimensions + 2) + 20;
      const y =
        500 - Math.ceil((this.noVotes + 1) / 15) * (this.squareDimensions + 2);
      return { x, y };
    },
    abstain() {
      const x =
        350 + (this.abstentions % 15) * (this.squareDimensions + 2) + 20;
      const y =
        500 -
        Math.ceil((this.abstentions + 1) / 15) * (this.squareDimensions + 2);
      return { x, y };
    },
    sortedVote() {
      return this.vote.positions.slice().sort((a, b) => {
        if (a.party < b.party) {
          return -1;
        } else if (b.party < a.party) {
          return 1;
        } else {
          return 0;
        }
      });
    }
  },
  mounted() {
    this.drawChart();
  },
  methods: {
    drawChart() {
      const yesGroup = this.$refs["yes-votes"];
      const noGroup = this.$refs["no-votes"];
      const abstainGroup = this.$refs["abstain-votes"];
      for (let i = 0; i < this.sortedVote.length; i++) {
        const currElement = this.sortedVote[i];
        const color = this.getColor(currElement.party);
        const { x, y } = this.getAndUpdateCoords(currElement["vote_position"]);
        // Create svg element and set attributes
        const square = document.createElementNS(
          "http://www.w3.org/2000/svg",
          "rect"
        );
        const title = this.generateTitle(currElement);
        square.appendChild(title);
        square.setAttribute("x", x);
        square.setAttribute("y", y);
        square.setAttribute("width", this.squareDimensions + "px");
        square.setAttribute("height", this.squareDimensions + "px");
        square.setAttribute("fill", color);
        square.setAttribute("class", "vote-square");
        // Attach element to proper group
        if (currElement.vote_position === "Yes") {
          yesGroup.append(square);
        } else if (currElement.vote_position === "No") {
          noGroup.append(square);
        } else {
          abstainGroup.append(square);
        }
      }
    },
    getColor(party) {
      let color = "gray";
      if (party == "R") {
        color = "red";
      } else if (party == "D") {
        color = "blue";
      }
      return color;
    },
    getAndUpdateCoords(position) {
      let currVal;
      if (position === "Yes") {
        currVal = this.yes;
        this.yesVotes += 1;
      } else if (position === "No") {
        currVal = this.no;
        this.noVotes += 1;
      } else {
        currVal = this.abstain;
        this.abstentions += 1;
      }
      return currVal;
    },
    generateTitle(currElement) {
      const title = document.createElementNS(
        "http://www.w3.org/2000/svg",
        "title"
      );
      let titleString = `Name: ${currElement.name}\nState: ${currElement.state}\nParty: ${currElement.party}\nVote Position: ${currElement.vote_position}\nDW Nominate: ${currElement.dw_nominate}`;
      title.textContent = titleString;
      return title;
    }
  }
};
</script>

<style>
.vote-square {
  cursor: pointer;
}

#vote-chart {
  max-height: 550px;
  max-width: 1000px;
}
</style>
