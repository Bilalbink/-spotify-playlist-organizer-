<template>
  <div>
    <b-navbar toggleable="md" variant="success" >
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
    <UserDetails  v-bind:user="user" v-bind:playlistSize="playlists.length"/>
    <CreatePlaylistModal v-bind:toggleModal="toggleModal" @toggle-modal="togglePlaylistModal" @create-playlist="createPlaylist"/>
    <PlaylistScroll @select-playlist="selectPlaylist" v-bind:playlists="playlists"/>
    <b-button @click="togglePlaylistModal" class="create-button">Create Playlist</b-button>
    <Searchbar v-bind:genres="genres" @sort-genre="sortByGenre"/>
    <PlaylistTable v-bind:songs="songs"/>
  </div>
</template>

<script>
import UserDetails from '../components/UserDetails';
import PlaylistScroll from '../components/PlaylistScroll';
import PlaylistTable from '../components/PlaylistTable';
import Searchbar from '../components/Searchbar';
import CreatePlaylistModal from '../components/CreatePlaylistModal'
import axios from 'axios';

export default {
  name: "Playlist",
  components: {
    UserDetails,
    PlaylistScroll,
    PlaylistTable,
    Searchbar,
    CreatePlaylistModal,
  },
  data() {
    return {
      playlists: [],
      user: null,
      songs: [],
      prevSongs: [],
      genres: [],
      toggleModal: false,
    }
  },
  methods: {
    selectPlaylist(playlist) {
      this.songs = [];
      this.prevSongs = [];
      const headers = 
        {
            "Content-Type": "application/json" ,
            "Authorization": "Bearer" + " " + localStorage.ACCESS_TOKEN 
        };
      for (let i = 0; i <= playlist.tracks.total; i += 100 ) {
        axios.get("https://api.spotify.com/v1/playlists/" + playlist.id + "/tracks?offset=" + i, {headers})
          .then((response) =>  {
              let data = response.data.items;
              // let newSongs = this.songs.concat(data);
              // this.songs = newSongs;

              let artistIds = [];
              response.data.items.forEach(item => {
                artistIds.push(item.track.artists[0].id)
              });
              
              if (artistIds.length < 50) {
                this.getGenre(artistIds, data);
              }
              else {
                this.getGenre(artistIds.slice(0, 50), data.slice(0, 50));
                this.getGenre(artistIds.slice(50, 100), data.slice(50, 100));
              }
          })
          .catch(function (error) {
              console.log(error);
          });
      }
    },
    getGenre(artistIds, songList) {
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
            data.forEach((artist, index) => {
              songList[index].track.genres = artist.genres;
              artist.genres.forEach(genre => {
                  if (genresList.indexOf(genre) === -1 & this.genres.indexOf(genre) === -1) {
                      genresList.push(genre);
                  }
              });
            });
            this.genres = this.genres.concat(genresList);
            this.songs = this.songs.concat(songList).sort(this.compareDateAdded);
            this.prevSongs = [...this.songs];
            // console.log(JSON.stringify(this.genres))
        })
        .catch(function (error) {
            console.log(error);
        });
    },
    compareDateAdded( song1, song2 ) {
        if ( song1.added_at > song2.added_at ){
            return -1;
        }
        if ( song1.added_at < song2.added_at ){
            return 1;
        }
        return 0;
    },
    sortByGenre(genre) {
      let tempSongs = [];
      this.prevSongs.forEach((song, index) => {
        if (song.track.genres.indexOf(genre) !== -1) {
          tempSongs.push(song);
        }
      });
      this.songs = [...tempSongs];
    },
    togglePlaylistModal() {
      this.toggleModal = !this.toggleModal;
    },
    createPlaylist(name, description) {
      this.toggleModal = false;
      const data = {
        name: name,
        description: description,
        public: true,
      }
      const headers = 
      {
          "Content-Type": "application/json" ,
          "Authorization": "Bearer" + " " + localStorage.ACCESS_TOKEN 
      };
      axios.post("https://api.spotify.com/v1/users/" + this.user.id + "/playlists", data, {headers})
      .then((response) =>  {
          this.addSongs(response.data.id);
      })
      .catch(function (error) {
          console.log(error);
      });
    },
    addSongs(playlistId) {
      let songUriList = encodeURIComponent(this.songs.map(song => {
        return(song.track.uri);
      }).join(","));
  
      const headers = 
        {
            "Content-Type": "application/json" ,
            "Authorization": "Bearer" + " " + localStorage.ACCESS_TOKEN 
        };
      axios.post("https://api.spotify.com/v1/playlists/" + playlistId + "/tracks?uris=" + songUriList, null, {headers})
      .then((response) =>  {
          console.log(response);

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
          console.log(data);
      })
      .catch(function (error) {
          console.log(error);
      });
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400;700&display=swap');

  .create-button {
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
      margin-bottom: 25px;
  }
  .create-button:focus {
    outline: none;
    border: none;
    border-color: inherit;
    -webkit-box-shadow: none;
    box-shadow: none;
  }
  .create-button:active {
    color: #fff !important;
    background-color: #4af772 !important;
    border-color: #4af772 !important;
    border-color: inherit !important;
    -webkit-box-shadow: none !important;
    box-shadow: none !important;
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
</style>