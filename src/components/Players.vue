<template>
  <div id="players">
    <v-layout>
      <v-flex xs6>
        <v-tabs slider-color="grey lighten-4">
          <v-tab color="">Create</v-tab>
          <v-tab-item>
            <v-card flat>
              <v-form>
                <v-container>
                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="name" label="Player name"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="wins" label="Player wins"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="losses" label="Player losses"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="race" label="Player race"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="realmId" label="Realm ID"></v-text-field>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-form>
              <v-btn @click="createPlayer">Submit</v-btn>
            </v-card>
          </v-tab-item>

          <v-tab>Delete</v-tab>
          <v-tab-item>
            <v-card flat>
              <v-form>
                <v-container>
                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="playerId" label="Player ID"></v-text-field>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-form>
              <v-btn @click="deletePlayer">Submit</v-btn>
            </v-card>
          </v-tab-item>

          <v-tab>Update</v-tab>
          <v-tab-item>
            <v-card flat>
              <v-form>
                <v-container>
                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="playerId" label="Player ID"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="name" label="Player name"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="wins" label="Player wins"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="losses" label="Player losses"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="race" label="Player race"></v-text-field>
                    </v-flex>
                  </v-layout>

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="realmId" label="Realm ID"></v-text-field>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-form>
              <v-btn @click="updatePlayer">Submit</v-btn>
            </v-card>
          </v-tab-item>
        </v-tabs>
      </v-flex>
    </v-layout>

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
      playerId: "",
      realmId: ""
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
                  _id: this.realmId
                }
              }
            }
          }
        })
        .then(result => {
          this.$apollo.queries.AllPlayers.refetch();
          console.log(result.data);
        })
        .catch(error => {
          console.log(error);
        });

        this.name = "";
        this.wins = 0;
        this.losses = 0;
        this.race = "";
        this.realmId = "";
    },
    deletePlayer() {
      this.$apollo
        .mutate({
          mutation: gql`
            mutation DeletePlayer($where: PlayerWhereUniqueInput!) {
              deletePlayer(where: $where) {
                _id
              }
            }
          `,
          variables: {
            where: {
              _id: this.playerId
            }
          }
        })
        .then(result => {
          this.$apollo.queries.players.refetch();
          console.log(result.data);
        })
        .catch(error => {
          console.log(error);
        });

        this.name = "";
        this.wins = 0;
        this.losses = 0;
        this.race = "";
        this.playerId = "";
    },
    updatePlayer() {
      this.$apollo
        .mutate({
          mutation: gql`
            mutation UpdatePlayer($data: PlayerUpdateInput!, $where: PlayerWhereUniqueInput!) {
              updatePlayer(data: $data, where: $where) {
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
                  _id: this.realmId
                }
              }
            },
            where: {
              _id: this.playerId
            }
          }
        })
        .then(result => {
          this.$apollo.queries.players.refetch();
          console.log(result.data);
        })
        .catch(error => {
          console.log(error);
        });

        this.playerId = "";
        this.name = "";
        this.wins = 0;
        this.losses = 0;
        this.race = "";
        this.realmId = "";
    }
  },
  apollo: {
    players: gql`
      query GetAllPlayers {
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
