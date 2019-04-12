<template>
  <div id="players">
    <ul>
      <li v-for="player in players" :key="player._id">{{ player.name }}</li>
    </ul>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  data() {
    return {
      players: [],
      name: "",
      wins: 0,
      losses: 0,
      race: "",
      realm: ""
    };
  },
  methods: {
    addPlayer() {
      //const playerName = this.playerName;
      const playerName = 'Test';
      const playerWins = 0;
      const playerLosses = 0;
      const playerRace = 'Human';
      const _id = '5ca1984ee03dd80007aa008e';

      this.apollo.mutate({
        mutation: gql`
          mutation ($name: String!, 
          wins: Int!, 
          losses: Int!, 
          race: String!, 
          _id: String!) {
            createPlayer(name: $name, 
            wins: $wins, 
            losses: $losses, 
            race: $race, 
            _id: $_id) {
              name
              wins
              losses
              race
              realm: {
                connect: {
                  _id
                }
              }
            }
          }`,
        variables: {
          name: playerName,
          wins: playerWins,
          losses: playerLosses,
          race: playerRace,
          _id: _id
        },
        update: (store, { data: { createPlayer }}) => {
          const data = store.readQuery({ query: PLAYERS_QUERY})
          data.players.push(createPlayer);
          store.writeQuery({ query: PLAYERS_QUERY, data });
        },
        optimisticResponse: {
          __typename: 'Mutation',
          createPlayer: {
            __typename: 'Player',
            id: -1,
            name: playerName,
            wins: playerWins,
            losses: playerLosses,
            race: playerRace,
            _id: _id
          }
        },

      }).then((data) => {
        console.log(data);
      }).catch((err) => {
        console.log(err);
        const playerName = '';
        const playerWins = 0;
        const playerLosses = 0;
        const playerRace = '';
        const _id = '';
      });
    }
  },
  apollo: {
    players: gql`
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
