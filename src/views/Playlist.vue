<template>
  <div class="about">
    <UserDetails/>
  </div>
</template>

<script>
import UserDetails from '../components/UserDetails.vue'
export default {
  name: "Playlist",
  components: {
    UserDetails
  },
  created () {
    console.log(localStorage.ACCESS_TOKEN);
  },
}
</script>

<style scoped>
</style>