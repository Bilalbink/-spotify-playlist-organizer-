<template>
    <div class="body">
      <b-container >
        <b-navbar toggleable="md" >
          <b-navbar-brand class="logo" href="/">
            SPO
            <b-icon-music-note-beamed class="music-icon-top"></b-icon-music-note-beamed>
          </b-navbar-brand>
          <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

          <b-collapse id="nav-collapse" is-nav>
              <b-navbar-nav class="ml-auto">
                 <b-nav-item href="https://github.com/Bilalbink" active>
                      <b-row align-h="center">
                          <b-icon-github class="github-icon"></b-icon-github>
                          <div class="github-name">Bilalbink</div>
                      </b-row>
                  </b-nav-item>
              </b-navbar-nav>
          </b-collapse>
        </b-navbar>
          <b-row align-h="center" class="web-icon">
            <b-icon-music-note-beamed class="music-icon"></b-icon-music-note-beamed>
          </b-row>
           <b-row align-h="center" class="spo">
             Spotify Playlist Organizer
           </b-row>
            <b-row align-h="center" class="web-desc">
             Log in to your Spotify account to start!
           </b-row>
          <b-row align-h="center" >
            <b-button @click="submit" class="button-login">Log In</b-button>
          </b-row>
      </b-container>
    </div>
</template>

<script>
import { BIcon, BIconGithub, BIconMusicNoteBeamed } from 'bootstrap-vue'

export default {
  name: "Login",
  components: {
    BIcon,
    BIconGithub,
    BIconMusicNoteBeamed
  },
  data () {
    return {
        }
  },
  methods: {
    submit() {
      let redirect_uri = encodeURIComponent('https://spo.vercel.app/callback/');
      let scopes = encodeURIComponent('playlist-modify-public playlist-modify-private playlist-read-private playlist-read-collaborative user-library-modify');
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
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400;700&display=swap');

.body {
    background-image: url('../assets/bgImage.jpg');
    background-position: center center;
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-size: cover;
    height: 100vh;
}
.logo {
  color: white;
  font-family: 'Raleway', sans-serif;
  font-weight: 700;
}
.nav-container {
  max-width: 1080px;
}
.github-icon{
  color: white;
  width: 25px;
  height: 25px;
}
.github-name {
  color: white;
  font-family: 'Raleway', sans-serif;
  font-weight: 700;
  margin-left: 10px;
  padding-top: 2px;
}
.web-icon {
  margin-top: 150px;
}
.web-desc {
  margin-top: 20px;
  color: white;
  font-family: 'Raleway', sans-serif;
  font-size: 15px;
}
.music-icon{
  color: white;
  width: 100px;
  height: 100px;
}
.music-icon-top {
  color: white;
  width: 25px;
  height: 25px;
}
.spo {
  color: white;
  font-family: 'Raleway', sans-serif;
  font-size: 25px;
  margin-top: 25px;
}
.button-login {
  margin-right: 15px;
  border-radius: 25px;
  min-width: 15%;
  height: 50px;
  outline: none;
  border: none;
  color: white;
  background-color: #17AA4A;
  font-family: 'Raleway', sans-serif;
  font-weight: 700;
  font-size: 20px;
  margin-top: 25px;
}
</style>