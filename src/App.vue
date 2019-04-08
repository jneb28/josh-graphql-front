<template>
  <v-app>
    <v-navigation-drawer app></v-navigation-drawer>
    <v-toolbar app></v-toolbar>
    <v-content>
      <v-container fluid>
        <router-view>
          <HelloWorld/>
        </router-view>
      </v-container>
    </v-content>
    <v-footer app></v-footer>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld'
//import axios from 'axios'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data () {
    return {
      //
    }
  }
  // mounted () {
  //   axios({
  //     url: 'http://localhost:4000/',
  //     method: 'post',
  //     data: {
  //       query: `
  //         query {
  //           players {
  //             name
  //           }
  //         }
  //         `
  //     }
  //   }).then((result) => {
  //     console.log(result.data)
  //   });
  // }
}
</script>
