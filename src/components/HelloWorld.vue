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

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="realm" label="Player realm"></v-text-field>
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

                  <v-layout>
                    <v-flex xs12>
                      <v-text-field v-model="realm" label="Player realm"></v-text-field>
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
              <v-btn depressed color="grey darken-2" @click="getRealm(realm)">Submit</v-btn>
            </v-card>
          </v-tab-item>

          <v-tab>Top 10</v-tab>
          <v-tab-item>
            <v-card flat>
              <v-form>
                <v-container>
                  <v-layout>
                    <v-flex xs12>
                      <v-btn depressed color="grey darken-2" @click="getTop10">Submit</v-btn>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-form>
            </v-card>
          </v-tab-item>
        </v-tabs>
      </v-flex>
    </v-layout>

    <v-layout row justify-center mb-2>
      <v-flex v-if="playerQuery.length !== 0" xs6>
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
            <v-card-text>Realm: {{ playerQuery.realm }}</v-card-text>
          </v-card>
        </v-card>
      </v-flex>

      <v-flex v-if="realmQuery.length !== 0" xs6>
        <v-card flat>
          <v-card-title class="grey darken-4" primary-title>
            <v-icon large left>language</v-icon>
            <div>
              <div class="headline">{{ realmQuery[0].realm }}</div>
            </div>
          </v-card-title>
          <v-card flat v-for="player in realmQuery" :key="player._id">
            <v-card-text>{{ player.name }}</v-card-text>
            <v-card-text>{{ player._id }}</v-card-text>
            <v-divider></v-divider>
          </v-card>
        </v-card>
      </v-flex>
    </v-layout>

    <v-layout justify-center row>
      <v-flex xs6>
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
import axios from "axios";

export default {
  data: () => ({
    players: [],
    playerQuery: [],
    realmQuery: [],
    name: "",
    wins: 0,
    losses: 0,
    race: "",
    playerId: "",
    realm: ""
  }),
  created() {
    const url = "http://localhost:3000/";
    axios
      .get(url)
      .then(res => {
        this.players = res.data;
        console.log(res.data);
      })
      .catch(err => console.log(err));
  },
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
  watch: {
    players: function(newPlayers, oldPlayers) {
      const url = "http://localhost:3000/";
      axios
        .get(url)
        .then(res => {
          this.players = res.data;
          console.log(res.data);
        })
        .catch(err => console.log(err));
    }
  },
  methods: {
    createPlayer() {
      const url = "http://localhost:3000/create-player";
      axios
        .post(url, {
          name: this.name,
          wins: Number(this.wins),
          losses: Number(this.losses),
          race: this.race,
          realm: this.realm
        })
        .then(function(res) {
          console.log(res);
        })
        .catch(function(error) {
          console.log(error);
        });

      this.realmQuery = [];
      this.playerQuery = [];
      this.name = "";
      this.wins = 0;
      this.losses = 0;
      this.race = "";
      this.realm = "";
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

      this.realmQuery = [];
      this.playerQuery = [];
      this.playerId = "";
    },
    updatePlayer(playerId) {
      const url = `http://localhost:3000/update-player/${playerId}`;
      axios
        .put(url, {
          name: this.name,
          wins: Number(this.wins),
          losses: Number(this.losses),
          race: this.race,
          realm: this.realm
        })
        .then(function(res) {
          console.log(res);
        })
        .catch(function(error) {
          console.log(error);
        });

      this.realmQuery = [];
      this.playerQuery = [];
      this.playerId = "";
      this.name = "";
      this.wins = 0;
      this.losses = 0;
      this.race = "";
      this.realm = "";
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

      this.realmQuery = [];
      this.playerQuery = [];
      this.playerId = "";
    },
    getRealm(realm) {
      const url = `http://localhost:3000/view-realm/${realm}`;
      axios
        .get(url)
        .then(res => {
          this.realmQuery = res.data;
          console.log(res.data);
        })
        .catch(err => console.log(err));

      this.realmQuery = [];
      this.playerQuery = [];
      this.realm = "";
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

      this.realmQuery = [];
      this.playerQuery = [];
    }
  }
};
</script>