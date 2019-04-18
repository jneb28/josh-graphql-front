<template>
  <div id="players">
    <div>
      <form method="POST">
        <input type="text" name="Name" v-model="name">
        <input type="number" name="Wins" v-model="wins">
        <input type="number" name="Losses" v-model="losses">
        <input type="text" name="Race" v-model="race">
        <input type="text" name="Realm" v-model="realmId">
      </form>
      <button @click="createPlayer">Create Player</button>
    </div>

    <p>Name: {{ name }}</p>
    <p>Wins: {{ wins }}</p>
    <p>Losses: {{ losses }}</p>
    <p>Race: {{ race }}</p>
    <p>Realm ID: {{ realmId }}</p>

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
          mutation createPlayer(
            $name: String!
            $wins: Int!
            $losses: Int!
            $race: String!
            $_id: String!
          ) {
            createPlayer(
              data: {
                name: $name
                wins: $wins
                losses: $losses
                race: $race
                realm: { connect: { _id: $_id } }
              }
            ) {
              _id
              name
              realm {
                name
              }
            }
          }
        `,
        variables: {
          name: name,
          wins: wins,
          losses: losses,
          race: race,
          _id: realmId
        }
      })
      .then(result => console.log(result))
      .catch(error => console.log(error));
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
