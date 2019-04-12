<template>
  <div class="about">
    <h1>This is an about page</h1>
    <Players/>
  </div>
</template>

<script>
import Players from '@/components/Players.vue'
import gql from 'graphql-tag'

export default {
  components: {
    Players
  }
}
</script>

