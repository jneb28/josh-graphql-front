<template>
<div id="players">
  <ul>
    <li v-for="player in players" :key="player._id">
      {{ player.name }}
    </li>
  </ul>
</div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      players: []
    };
  },
  async mounted() {
    try {
      var result = await axios({
        method: "POST",
        url: "http://localhost:4000/",
        data: {
          query: `
            {
              players {
                name
              }
            }
          `
        }
      });
      this.players = result.data.data.player;
    } catch(error) {
      console.log(error);
    }
  }
};
</script>

<style>
</style>
