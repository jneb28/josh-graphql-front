<template>
  <v-app dark>
    <v-toolbar flat app>
      <v-toolbar-title>Player Records</v-toolbar-title>
      <v-spacer></v-spacer>
        <v-toolbar-items class="hidden-sm-and-down">
          <v-btn flat to="/">REST</v-btn>
          <v-btn flat to="/about">GraphQL</v-btn>
        </v-toolbar-items>
    </v-toolbar>

    <v-content>
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-content>
    
    <v-footer app></v-footer>
  </v-app>
</template>