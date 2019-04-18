<template>
  <div id="players">
    <div>
      <form>
        <div>
          <span>Name:</span>
          <input type="text" name="Name" v-model="name">
        </div>
        <div>
          <span>Wins:</span>
          <input type="text" name="Wins" v-model="wins">
        </div>
        <div>
          <span>Losses:</span>
          <input type="text" name="Losses" v-model="losses">
        </div>
        <div>
          <span>Race:</span>
          <input type="text" name="Race" v-model="race">
        </div>
        <div>
          <span>Realm ID:</span>
          <input type="text" name="Realm" v-model="id">
        </div>
      </form>
      <button @click="createPlayer">Create Player</button>
    </div>

    <p>Name: {{ name }}</p>
    <p>Wins: {{ wins }}</p>
    <p>Losses: {{ losses }}</p>
    <p>Race: {{ race }}</p>
    <p>Realm ID: {{ id }}</p>

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
      id: ""
      
    };
  },
  methods: {
    createPlayer() {
      this.$apollo
        .mutate({
          mutation: gql`
           mutation CreatePlayer($data: PlayerCreateInput!) {
             createPlayer(data: $data) {
               _id
             }
           }
          `,
          variables: {
            data: {
              name: this.name,
              wins: Number(this.wins),
              losses: Number(this.losses),
              race: this.race,
              realm: {
                connect: {
                  _id: this.id
                }
              }
            }
          }
        })
        .then(result => {
          console.log(result.data);
        })
        .catch(error => {
          console.log(error);
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

<style scoped>
input {
  border: 1px solid #f2f2f2;
}
</style>
