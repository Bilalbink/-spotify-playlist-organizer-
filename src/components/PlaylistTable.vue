<template>
    <b-container fluid="xl">
        <b-table-simple hover responsive>
            <b-thead>
                <b-tr>
                    <b-th>#</b-th>
                    <b-th>Title</b-th>
                    <b-th>Artists</b-th>
                    <b-th>Album</b-th>
                    <b-th>Genre</b-th>
                </b-tr>
            </b-thead>
            <b-tbody v-if="songs">
                <b-tr v-for="(song, index) in songs" :key="song.name">
                    <b-td>{{index + 1}}</b-td>
                    <b-td>{{song.track.name}}</b-td>
                    <b-td>{{song.track.artists.map(artist => {return artist.name;}).join(", ")}}</b-td>
                    <b-td>{{song.track.album.name}}</b-td>
                    <b-td v-if="genres">{{genres[index].map(genre => {return genre;}).join(", ")}}</b-td>
                </b-tr>.
            </b-tbody>
    </b-table-simple>
  </b-container>
</template>

<script>
export default {
    name: 'PlaylistTable',
    props: {
        songs: Array,
        genres: Array,
    }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400;700&display=swap');

.table {
    border:0px solid black;
    border-bottom: 1px solid #282828;
    margin-top:20px;
    background-color: #181818;
}
.table > tbody > tr > td {
    border:0px solid black;
    border-bottom: 1px solid #282828;
    background-color: #181818;
    color: white;
    font-family: 'Raleway', sans-serif;
}
.table thead > tr > th {
    border:0px solid black;
    border-bottom: 1px solid #282828;
    background-color: #17AA4A;
    cursor: pointer;
    color: white;
    font-family: 'Raleway', sans-serif;
}
</style>