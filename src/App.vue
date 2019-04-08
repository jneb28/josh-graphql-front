<template>
  <v-app dark>
    <v-navigation-drawer app>

      <v-toolbar>
        <v-list>
          <v-list-tile>
            <v-list-tile-title class="title">
              Navigation
            </v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-toolbar>

      <v-divider></v-divider>

      <v-list>
        <v-list-tile>
          <v-list-tile-content>
            <v-list-tile-title>
              <router-link to="/">Home</router-link>
            </v-list-tile-title>
            <v-list-tile-title>
              <router-link to="/about">About</router-link>
            </v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
      
    </v-navigation-drawer>

    <v-toolbar app></v-toolbar>

    <v-content>
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-content>
    
    <v-footer app></v-footer>
  </v-app>
</template>