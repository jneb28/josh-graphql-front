<template>
  <div id="players">
    <v-layout justify-center mb-5>
      <v-flex xs6>
        <v-tabs card color="grey darken-4" slider-color="grey lighten-4">
          <v-tab color>Create</v-tab>
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
                      <v-text-field v-model="realm" label="Realm Name"></v-text-field>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-form>
              <v-btn depressed color="grey darken-2" @click="createPlayer">Submit</v-btn>
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
              <v-btn depressed color="grey darken-2" @click="deletePlayer">Submit</v-btn>
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
                      <v-text-field v-model="realm" label="Realm Name"></v-text-field>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-form>
              <v-btn depressed color="grey darken-2" @click="updatePlayer">Submit</v-btn>
            </v-card>
          </v-tab-item>

          <v-tab>Find Player</v-tab>
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
              <v-btn depressed color="grey darken-2" @click="findPlayer">Submit</v-btn>
            </v-card>
          </v-tab-item>

          <v-tab>Find Realm</v-tab>
          <v-tab-item>
            <v-card flat>
              <v-form>
                <v-container>
                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="realm" label="Realm Name"></v-text-field>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-form>
              <v-btn depressed color="grey darken-2" @click="findRealm">Submit</v-btn>
            </v-card>
          </v-tab-item>
        </v-tabs>
      </v-flex>
    </v-layout>

    <v-layout row justify-center mb-2>
      <v-flex v-if="playerQuery.length !== 0" xs6>
        <v-card flat v-for="player in playerQuery" :key="player._id">
          <v-card-title class="grey darken-4" primary-title>
            <v-icon large left>account_circle</v-icon>
            <div>
              <div class="headline">{{ player.name }}</div>
              <span class="grey--text">{{ player._id }}</span>
            </div>
          </v-card-title>
          <v-card flat>
            <v-card-text>Wins: {{ player.wins }}</v-card-text>
            <v-card-text>Losses: {{ player.losses }}</v-card-text>
            <v-card-text>Race: {{ player.race }}</v-card-text>
            <v-card-text>Realm: {{ player.realm }}</v-card-text>
          </v-card>
        </v-card>
      </v-flex>

      <v-flex v-if="realmQuery.length !== 0" xs6>
        <v-card flat v-for="player in realmQuery" :key="player._id">
          <v-card-title class="grey darken-4" primary-title>
            <v-icon large left>language</v-icon>
            <div>
              <div class="headline">{{ player[0].realm }}</div>
            </div>
          </v-card-title>
          <v-list two-line v-for="data in player" :key="data._id">
            <v-card-text>{{ data.name }}</v-card-text>
            <v-card-text>{{ data._id }}</v-card-text>
            <v-divider></v-divider>
          </v-list>
        </v-card>
      </v-flex>
    </v-layout> 

    <v-layout row justify-center>
      <v-flex v-if="$apollo.queries.players.loading" xs6>
        <span>Loading players...</span>
      </v-flex>
      <v-flex v-else xs6>
        <v-card flat>
          <v-toolbar flat color="grey darken-4" dark>
            <v-toolbar-title>Players</v-toolbar-title>
          </v-toolbar>
          <v-list v-for="player in players" :key="player._id">
            <v-list-group>
              <template v-slot:activator>
                <v-card-title primary-title>
                  <v-icon large left>account_circle</v-icon>
                  <div>
                    <div class="headline">{{ player.name }}</div>
                    <span class="grey--text">{{ player._id }}</span>
                  </div>
                </v-card-title>
              </template>
              <v-card>
                <v-card-text>Wins: {{ player.wins }}</v-card-text>
                <v-card-text>Losses: {{ player.losses }}</v-card-text>
                <v-card-text>Race: {{ player.race }}</v-card-text>
                <v-card-text>Realm: {{ player.realm }}</v-card-text>
              </v-card>
            </v-list-group>
          </v-list>
        </v-card>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  data() {
    return {
      players: [],
      playerQuery: [],
      realmQuery: [],
      playerId: "",
      name: "",
      wins: 0,
      losses: 0,
      race: "",
      realm: "",
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
              realm: this.realm
            }
          }
        })
        .then(res => {
          this.$apollo.queries.players.refetch();
        })
        .catch(err => {
          console.log(err)
        });

      this.playerQuery = [];
      this.realmQuery = [];
      this.name = "";
      this.wins = 0;
      this.losses = 0;
      this.race = "";
      this.realm = "";
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
        .then(res => {
          this.$apollo.queries.players.refetch();
          console.log(res.data);
        })
        .catch(err => {
          console.log(err);
        });

      this.playerQuery = [];
      this.realmQuery = [];
      this.playerId = "";
    },
    updatePlayer() {
      this.$apollo
        .mutate({
          mutation: gql`
            mutation UpdatePlayer(
              $data: PlayerUpdateInput!
              $where: PlayerWhereUniqueInput!
            ) {
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
              realm: this.realm
            },
            where: {
              _id: this.playerId
            }
          }
        })
        .then(res => {
          this.$apollo.queries.players.refetch();
          console.log(res.data);
        })
        .catch(err => {
          console.log(err);
        });

      this.playerQuery = [];
      this.realmQuery = [];
      this.playerId = "";
      this.name = "";
      this.wins = 0;
      this.losses = 0;
      this.race = "";
      this.realm = "";
    },
    findPlayer() {
      this.$apollo
        .query({
          query: gql`
            query FindPlayer($where: PlayerWhereUniqueInput!) {
              player(where: $where) {
                _id
                name
                wins
                losses
                race
                realm
              }
            }
          `,
          variables: {
            where: {
              _id: this.playerId
            }
          }
        })
        .then(res => {
          this.playerQuery = res.data;
        })
        .catch(err => {
          console.log(err);
        });

      this.playerQuery = [];
      this.realmQuery = [];
      this.playerId = "";
    },
    findRealm() {
      this.$apollo
        .query({
          query: gql`
            query FindRealm($where: PlayerWhereInput) {
              players(where: $where) {
                _id
                name
                realm
              }
            }
          `,
          variables: {
            where: {
              realm: this.realm
            }
          }
        })
        .then(res => {
          this.realmQuery = res.data;
          console.log(res.data.players);
        })
        .catch(err => {
          console.log(err);
        });

      this.playerQuery = [];
      this.realmQuery = [];
      this.realm = "";
    }
  },
  apollo: {
    players: gql`
      query GetAllPlayers {
        players {
          _id
          name
          wins
          losses
          race
          realm
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
