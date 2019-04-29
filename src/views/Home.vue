<template>
  <div class="home">
    <h4 class="display-1">REST</h4>
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  components: {
    HelloWorld
  }
}
</script>