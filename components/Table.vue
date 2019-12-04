<template>
  <table class="vote-table">
    <thead>
      <tr>
        <th>Date</th>
        <th>Bill Title</th>
        <th>Vote Type</th>
        <th>Result</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(vote, idx) in votes" :key="idx">
        <td>{{ vote.date }}</td>
        <td>
          <nuxt-link :to="`vote/?${vote.queryString}`">{{
            vote.shortTitle
          }}</nuxt-link>
        </td>
        <td style="text-transform: capitalize;">
          {{ vote.vote_type.toLowerCase() }}
        </td>
        <td>{{ vote.result }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    votes: {
      type: Array,
      default: () => {}
    }
  }
};
</script>

<style lang="scss" scoped>
.vote-table {
  border-collapse: collapse;
  color: black !important;
  font-size: 14px;
  table-layout: fixed;
  text-align: center;
  text-decoration: none;
  width: 100%;

  th,
  td {
    padding-bottom: 20px;
    padding-top: 20px;
    text-overflow: ellipsis;
  }

  th {
    border-bottom: 1px solid black;
    font-size: 16px;
  }

  tr {
    :first-child {
      text-align: left;
    }

    :last-child {
      text-align: right;
    }
  }

  tr + tr {
    td {
      border-top: 1px solid black;
    }
  }
}
</style>
