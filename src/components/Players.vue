<template>
  <div id="players">

    <form action="Players.vue" method="post">
      <input type="text" name="name" v-model="name">
      <input type="num" name="wins" v-model="wins">
      <input type="num" name="losses" v-model="losses">
      <input type="text" name="race" v-model="race">
      <input type="text" name="realm" v-model="realmId">
      <input type="submit" name="submit">
    </form>

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
      const query = gql`
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
      `;
      
      const {
        name,
        wins,
        losses,
        race,
        realmId
      } = this.data;

      this.apollo.mutate( {
        mutation: query,
        variables: {
          name,
          wins,
          losses,
          race,
          realmId
        }
      })
      
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
