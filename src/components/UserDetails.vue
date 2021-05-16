<template>
  <div>
    <b-container>
      <b-row align-v="center">
        <b-col md="auto">
          <img v-bind:src="user.images[0].url" alt="Profile Image">
        </b-col>
        <b-col md="auto" id="desc">
          <b-row id='username'>
              {{user.display_name}}
          </b-row>
          <b-row id='playlistDetails'>
              Playlists: 5
          </b-row >
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name: "UserDetails",
    data() {
        return {
            user: null
        }
    },
    created() {
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
    }
}
</script>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400;700&display=swap');
  
  img {
    border-radius: 50%;
    height: 200px;
    width: 200px;
    object-fit: cover;
    border: 5px solid #17AA4A;
  }

  #desc {
      margin-left: 25px;
  }
  #username {
    color: white;
    font-family: 'Raleway', sans-serif;
    font-weight: 700;
    font-size: 75px;
  }

  #playlistDetails {
    color: white;
    font-family: 'Raleway', sans-serif;
    font-weight: 300;
    padding-left: 5px;
  }

  div {
      margin-top: 25px;
  }

</style>