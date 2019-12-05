<template>
  <table class="vote-table">
    <thead>
      <tr>
        <th class="sticky-header">Date</th>
        <th class="sticky-header">Bill Title</th>
        <th class="sticky-header">Vote Type</th>
        <th class="sticky-header">Result</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(vote, idx) in votes" :key="idx">
        <td>
          <nuxt-link :to="`vote/?${vote.queryString}`">{{
            vote.date
          }}</nuxt-link>
        </td>
        <td>
          <nuxt-link :to="`vote/?${vote.queryString}`">{{
            vote.shortTitle
          }}</nuxt-link>
        </td>
        <td style="text-transform: capitalize;">
          <nuxt-link :to="`vote/?${vote.queryString}`">
            {{ vote.vote_type.toLowerCase() }}
          </nuxt-link>
        </td>
        <td>
          <nuxt-link :to="`vote/?${vote.queryString}`">{{
            vote.result
          }}</nuxt-link>
        </td>
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
  height: 800px;
  overflow: scroll;
  table-layout: fixed;
  text-align: center;
  text-decoration: none;
  width: 100%;

  .sticky-header {
    background: white;
    border-bottom: 1px solid black;
    box-shadow: 0 1px 0px 0px rgba(0, 0, 0, 1);
    position: sticky;
    top: 70px;
  }

  th,
  td {
    a {
      padding-bottom: 20px;
      padding-top: 20px;
      text-overflow: ellipsis;
      color: black;
      display: block;
      height: 100%;
      text-decoration: none;
      width: 100%;
    }
  }

  th {
    border-bottom: 1px solid black;
    font-size: 16px;
  }

  tr {
    cursor: pointer;
    &:hover {
      background: #f3f3f3;
    }

    :first-child a,
    th:first-child {
      text-align: left;
    }

    :last-child a,
    th:last-child {
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
