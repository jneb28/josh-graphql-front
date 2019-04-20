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
                      <v-text-field v-model="realmId" label="Realm ID"></v-text-field>
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
                      <v-text-field v-model="realmId" label="Realm ID"></v-text-field>
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
                      <v-text-field v-model="realmId" label="Realm ID"></v-text-field>
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

    <v-layout row mb-2>
      <v-flex v-if="playerQuery" xs6 mr-1>
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
            <v-card-text>Realm ID: {{ player.realm._id }}</v-card-text>
            <v-card-text>Realm Name: {{ player.realm.name }}</v-card-text>
          </v-card>
        </v-card>
      </v-flex>

      <v-flex v-if="realmQuery" xs6 ml-1>
        <v-card flat v-for="realm in realmQuery" :key="realm._id">
          <v-card-title class="grey darken-4" primary-title>
            <v-icon large left>language</v-icon>
            <div>
              <div class="headline">{{ realm.name }}</div>
              <span class="grey--text">{{ realm._id }}</span>
            </div>
          </v-card-title>
          <v-card flat>
            <v-card-text class="title">Population:</v-card-text>
            <v-list v-for="pop in realm.population" :key="pop._id" two-line>
              <v-list-tile>
                <v-list-tile-content>
                  <v-card-text>{{ pop.name }}</v-card-text>
                </v-list-tile-content>
              </v-list-tile>
            </v-list>
          </v-card>
        </v-card>
      </v-flex>
    </v-layout>

    <v-layout row>
      <v-flex v-if="$apollo.queries.players.loading" xs6 mr-1>
        <span>Loading players...</span>
      </v-flex>
      <v-flex v-else xs6 mr-1>
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
                <v-card-text>Realm: {{ player.realm.name }}</v-card-text>
              </v-card>
            </v-list-group>
          </v-list>
        </v-card>
      </v-flex>

      <v-flex v-if="$apollo.queries.realms.loading" xs6 ml-1>
        <span>Loading realms...</span>
      </v-flex>
      <v-flex v-else xs6 ml-1>
        <v-card flat>
          <v-toolbar flat color="grey darken-4" dark>
            <v-toolbar-title>Realms</v-toolbar-title>
          </v-toolbar>
          <v-list v-for="realm in realms" :key="realm._id">
            <v-list-group>
              <template v-slot:activator>
                <v-card-title primary-title>
                  <v-icon large left>language</v-icon>
                  <div>
                    <div class="headline">{{ realm.name }}</div>
                    <span class="grey--text">{{ realm._id }}</span>
                  </div>
                </v-card-title>
              </template>
              <v-card>
                <v-card-text class="title">Population:</v-card-text>
                <v-list v-for="pop in realm.population" :key="pop._id" two-line>
                  <v-list-tile>
                    <v-list-tile-content>
                      <v-card-text>{{ pop.name }}</v-card-text>
                    </v-list-tile-content>
                  </v-list-tile>
                </v-list>
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
      realms: [],
      playerQuery: [],
      realmQuery: [],
      name: "",
      wins: 0,
      losses: 0,
      race: "",
      playerId: "",
      realmId: "",
      result: "",
      error: ""
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
                realm {
                  _id
                  name
                }
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
          this.playerQuery = result.data;
          console.log(result.data);
        })
        .catch(error => {
          console.log(error);
        });

      this.playerId = "";
    },
    findRealm() {
      this.$apollo
        .query({
          query: gql`
            query FindRealm($where: RealmWhereUniqueInput!) {
              realm(where: $where) {
                _id
                name
                population {
                  _id
                  name
                }
              }
            }
          `,
          variables: {
            where: {
              _id: this.realmId
            }
          }
        })
        .then(result => {
          this.realmQuery = result.data;
          console.log(result.data);
        })
        .catch(error => {
          console.log(error);
        });

      this.realmId = "";
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
          realm {
            name
          }
        }
      }
    `,
    realms: gql`
      query GetAllRealms {
        realms {
          _id
          name
          population {
            _id
            name
          }
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
