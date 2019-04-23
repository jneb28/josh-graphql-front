<template>
  <div id="helloworld">
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
              <v-btn depressed color="grey darken-2" @click="deletePlayer(playerId)">Submit</v-btn>
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
                </v-container>
              </v-form>
              <v-btn depressed color="grey darken-2" @click="updatePlayer(playerId)">Submit</v-btn>
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
              <v-btn depressed color="grey darken-2" @click="getPlayer(playerId)">Submit</v-btn>
            </v-card>
          </v-tab-item>

          <v-tab>Top 10</v-tab>
          <v-tab-item>
            <v-card flat>
              <v-form>
                <v-container>
                  <v-layout>
                    <v-flex xs12>
                      <!-- <v-text-field v-model="realmId" label="Realm ID"></v-text-field> -->
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-form>
              <v-btn depressed color="grey darken-2" @click="getTop10">Submit</v-btn>
            </v-card>
          </v-tab-item>
        </v-tabs>
      </v-flex>
    </v-layout>

    <v-layout justify-center row mb-2>
      <v-flex v-if="playerQuery.length !== 0" xs6 mr-1>
        <v-card flat>
          <v-card-title class="grey darken-4" primary-title>
            <v-icon large left>account_circle</v-icon>
            <div>
              <div class="headline">{{ playerQuery.name }}</div>
              <span class="grey--text">{{ playerQuery._id }}</span>
            </div>
          </v-card-title>
          <v-card flat>
            <v-card-text>Wins: {{ playerQuery.wins }}</v-card-text>
            <v-card-text>Losses: {{ playerQuery.losses }}</v-card-text>
            <v-card-text>Race: {{ playerQuery.race }}</v-card-text>
          </v-card>
        </v-card>
      </v-flex>
    </v-layout>

    <v-layout justify-center row>
      <v-flex xs6 mr-1>
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
              </v-card>
            </v-list-group>
          </v-list>
        </v-card>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
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
  }),
  mounted() {
    const url = "http://localhost:3000/";
    axios
      .get(url)
      .then(res => {
        this.players = res.data;
        console.log(res.data);
      })
      .catch(err => console.log(err));
  },
  methods: {
    createPlayer() {
      const url = "http://localhost:3000/create-player";
      axios
        .post(url, {
          name: this.name,
          wins: Number(this.wins),
          losses: Number(this.losses),
          race: this.race
        })
        .then(function(res) {
          console.log(res);
        })
        .catch(function(error) {
          console.log(error);
        });

      this.name = "";
      this.wins = 0;
      this.losses = 0;
      this.race = "";
      this.realmId = "";
    },
    deletePlayer(playerId) {
      const url = `http://localhost:3000/delete-player/${playerId}`;
      axios
        .delete(url)
        .then(function(res) {
          console.log(res);
        })
        .catch(function(error) {
          console.log(error);
        });

      this.playerId = "";
    },
    updatePlayer(playerId) {
      const url = `http://localhost:3000/update-player/${playerId}`;
      axios
        .put(url, {
          name: this.name,
          wins: Number(this.wins),
          losses: Number(this.losses),
          race: this.race
        })
        .then(function(res) {
          console.log(res);
        })
        .catch(function(error) {
          console.log(error);
        });

      this.playerId = "";
      this.name = "";
      this.wins = 0;
      this.losses = 0;
      this.race = "";
    },
    getPlayer(playerId) {
      const url = `http://localhost:3000/view-player/${playerId}`;
      axios
        .get(url)
        .then(res => {
          this.playerQuery = res.data;
          console.log(res.data);
        })
        .catch(err => console.log(err));

      this.playerId = "";
    },
    getTop10() {
      const url = "http://localhost:3000/top10";
      axios
        .get(url)
        .then(res => {
          this.players = res.data;
          console.log(res.data);
        })
        .catch(err => console.log(err));
    }
  }
};
</script>