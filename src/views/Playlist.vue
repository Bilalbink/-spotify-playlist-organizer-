<template>
  <div>
    <UserDetails  v-bind:user="user" v-bind:playlistSize="playlists.length"/>
    <PlaylistScroll @select-playlist="selectPlaylist" v-bind:playlists="playlists"/>
    <PlaylistTable v-bind:songs="songs" v-bind:genres="genres"/>
  </div>
</template>

<script>
import UserDetails from '../components/UserDetails.vue';
import PlaylistScroll from '../components/PlaylistScroll';
import PlaylistTable from '../components/PlaylistTable'
import axios from 'axios'

export default {
  name: "Playlist",
  components: {
    UserDetails,
    PlaylistScroll,
    PlaylistTable
  },
  data() {
    return {
      playlists: [],
      user: null,
      songs: [],
      genres: [],
    }
  },
  methods: {
    selectPlaylist(playlist) {
      this.songs = [];
      this.genres = [];
      const headers = 
        {
            "Content-Type": "application/json" ,
            "Authorization": "Bearer" + " " + localStorage.ACCESS_TOKEN 
        };
      for (let i = 0; i <= playlist.tracks.total; i += 100 ) {
        axios.get("https://api.spotify.com/v1/playlists/" + playlist.id + "/tracks?offset=" + i, {headers})
          .then((response) =>  {
              let data = response.data.items;
              let newSongs = this.songs.concat(data);
              this.songs = newSongs;

              let artistIds = [];
              response.data.items.forEach(item => {
                artistIds.push(item.track.artists[0].id)
              });
              
              if (artistIds.length < 50) {
                this.getGenre(artistIds);
              }
              else {
                this.getGenre(artistIds.slice(0, 50));
                this.getGenre(artistIds.slice(50, 100));
              }
          })
          .catch(function (error) {
              console.log(error);
          });
      }
    },
    getGenre(artistIds) {
      artistIds = encodeURIComponent(artistIds.toString());
      const headers = 
      {
          "Content-Type": "application/json" ,
          "Authorization": "Bearer" + " " + localStorage.ACCESS_TOKEN 
      };
      axios.get("	https://api.spotify.com/v1/artists?ids=" + artistIds, {headers})
        .then((response) =>  {
            let data = response.data.artists;
            let genresList = [];
            console.log(data);
            data.forEach(artist => {
              genresList.push(artist.genres);
            })
            this.genres = this.genres.concat(genresList);
        })
        .catch(function (error) {
            console.log(error);
        });
    }
  },
  created () {
    const headers = 
    {
        "Content-Type": "application/json" ,
        "Authorization": "Bearer" + " " + localStorage.ACCESS_TOKEN 
    };
    axios.get("https://api.spotify.com/v1/me", {headers})
      .then((response) =>  {
          let data = response.data;
          this.user = data;
      })
      .catch(function (error) {
          console.log(error);
      });
    axios.get("https://api.spotify.com/v1/me/playlists", {headers})
      .then((response) =>  {
          let data = response.data.items.filter(song => song.owner.display_name === this.user.display_name);
          this.playlists = data;
      })
      .catch(function (error) {
          console.log(error);
      });
  },
}
</script>

<style scoped>
</style>