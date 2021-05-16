<template>
  <div>
    <b-container class="bv-example-row">
      <b-row>
        <b-col>
          <b-form-input v-model="clientId" placeholder="Enter Client ID"></b-form-input>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <b-button @click="submit" variant="success">Enter</b-button>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Login",
  data () {
    return {
      clientId: ''
    }
  },
  methods: {
    submit() {
      // let spotify_url = 'https://accounts.spotify.com/authorize';
      let redirect_uri = encodeURIComponent('http://localhost:8080/callback/');
      let scopes = encodeURIComponent('playlist-modify-public playlist-modify-private user-library-modify');
      window.location.replace('https://accounts.spotify.com/authorize?' + 
        'client_id=' + '6650d8efd20648c68a8d6f22dc1ac7b0' + 
        '&redirect_uri=' + redirect_uri + 
        '&scope=' + scopes + 
        '&response_type=token'
      )
    }
  },
  components: {},
};
</script>

<style scoped>
  div {
    background-color: #121212;
  }
</style>