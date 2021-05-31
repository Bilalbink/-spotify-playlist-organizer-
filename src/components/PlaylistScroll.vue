<template>
    <div>
        <div class="playlist-card">
            <h1>Select Playlist</h1>
            <vue-horizontal-list :items="playlists" :options="options">
                <template v-slot:default="{ item }">
                    <div @click="$emit('select-playlist', item)" class="item">
                        <img v-if="item.images.length !== 0" class="playlist-image" v-bind:src="item.images[0].url" alt="Profile Image">
                        <img v-else class="playlist-image" src="@/assets/defaultcover.jpg" alt="Profile Image">
                        <p>{{ item.name }}</p>
                    </div>
                </template>
            </vue-horizontal-list>
        </div>
    </div>
</template>

<script>
import VueHorizontalList from "../../node_modules/vue-horizontal-list/src/vue-horizontal-list";

export default {
    name: "PlaylistScroll",
    components: {
        VueHorizontalList,
    },
    props: {
        playlists: Array
    },
    data() {
        return {
            options: {
                position: {
                    start: 0,
                },
            },
        }
    },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400;700&display=swap');
.playlist-card {
  max-width: 1400px;
  margin-left: auto;
  margin-right: auto;
  padding: 80px 24px;
}

.playlist-image {
    width: 200px;
    height: 200px;
    border-radius: 5px;
}
.item {
    background-color: #1E1E1E;
    border-radius: 5px;
    padding: 10px;
}
.item:hover {
    cursor: pointer;
    background-color: #282828;
}
p {
    margin-top: 20px;
    color: white;
    font-weight: 700;
    font-family: 'Raleway', sans-serif;
}
h1 {
    color: white;
    font-weight: 700;
    font-family: 'Raleway', sans-serif;
}
</style>