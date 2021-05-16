<template>
  <div>
    <UserDetails  v-bind:user="user" v-bind:playlistSize="playlists.length"/>
    <PlaylistScroll @select-playlist="selectPlaylist" v-bind:playlists="playlists"/>
    <PlaylistTable v-bind:songs="songs"/>
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
      genre: [],
    }
  },
  methods: {
    selectPlaylist(playlist) {
      this.songs = [];
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
              // console.log(this.songs);

              let artistIds = [];
              response.data.items.forEach(item => {
                artistIds.push(item.track.artists[0].id)
              });
              
              if (artistIds.length < 50) {
                    getGenre(artistIds);
                }
                else {
                    getGenre(artistIds.slice(0, 50));
                    getGenre(artistIds.slice(50, 100));
                }
          })
          .catch(function (error) {
              console.log(error);
          });
      }
    },
    getGenre(artistIds) {
      const headers = 
      {
          "Content-Type": "application/json" ,
          "Authorization": "Bearer" + " " + localStorage.ACCESS_TOKEN 
      };
      axios.get("	https://api.spotify.com/v1/artists?ids=", {headers})
        .then((response) =>  {
            let data = response.data;
            this.user = data;
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
        console.log(response.data.items[0].owner.display_name);
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