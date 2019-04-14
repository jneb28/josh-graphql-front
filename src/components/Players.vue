<template>
  <div id="players">
    <div>
      <input type="text" name="Name" v-model="name">
      <input type="number" name="Wins" v-model="wins">
      <input type="number" name="Losses" v-model="losses">
      <input type="text" name="Race" v-model="race">
      <input type="text" name="Realm" v-model="realmId">
      <button @click="createPlayer()">Create Player</button>
    </div>

    <p>{{ name }}</p>
    <p>{{ wins }}</p>
    <p>{{ losses }}</p>
    <p>{{ race }}</p>
    <p>{{ realmId }}</p>

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
      realmId: ""
    };
  },
  methods: {
    createPlayer() {
      const name = this.name;
      const wins = this.wins;
      const losses = this.losses;
      const race = this.race;
      const realmId = this.realmId;

      this.name = "";
      this.wins = 0;
      this.losses = 0;
      this.race = "";
      this.realmId = "";

      this.$apollo.mutate({
        mutation: gql`
          mutation CreatePlayer(
            $pName: String!
            $pWins: Int!
            $pLosses: Int!
            $pRace: String!
            $pRealmId: String!
          ) {
            createPlayer(
              data: {
                name: $pName
                wins: $pWins
                losses: $pLosses
                race: $pRace
                realm: { connect: { _id: $pRealmId } }
              }
            ) {
              _id
              name
              wins
              losses
              race
              realm {
                name
              }
            }
          }
        `,
        variables: {
          name,
          wins,
          losses,
          race,
          realmId
        }
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
