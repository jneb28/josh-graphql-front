<template>
<div id="players">
  <ul>
    <li v-for="player in players" :key="player._id">
      {{ player.name }}
    </li>
  </ul>
</div>
</template>

<script>
import gql from 'graphql-tag'

export default {
  data() {
    return {
      players: []
    };
  },
  apollo: {
    players: gql `
      query {
        players {
          name
        }
      }
    `
  }
};
</script>

<style>
</style>
