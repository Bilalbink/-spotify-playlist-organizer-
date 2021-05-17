<template>
<div>
    <b-container fluid="xl">
        <vue-bootstrap-typeahead
            :data="genres"
            v-model="text"
            textVarian=".text-light"
            placeholder="Search for a genre"
            @hit="$emit('sort-genre', $event)"
        />
        <div>{{this.selectedGenre}}</div>
    </b-container>
</div>
</template>

<script>
import VueBootstrapTypeahead from 'vue-bootstrap-typeahead'

export default {
    name: 'Searchbar',
    components: {
        VueBootstrapTypeahead
    },
    props: {
        genres: Array,
    },
    data() {
        return {
            selectedGenre: '',
            text: '',
        }
    }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400;700&display=swap');

    div {
        margin-bottom: 25px;
    }
    ::v-deep input {
        background-color: #121212;
        border-radius: 0;
        border: none;
        outline: none;
        border-bottom: 1px solid white;
        color: white;
        font-family: 'Raleway', sans-serif;
    }

    ::v-deep input:focus {
        background-color: #121212;
        border-radius: 0;
        border: none;
        outline: none;
        border-bottom: 1px solid white;
        color: white;        
        border-color: inherit;
        -webkit-box-shadow: none;
        box-shadow: none;
    }

    ::v-deep .list-group-item {
        background-color: #121212;
        border: none;
        outline: none;
        border-bottom: 1px solid rgb(63, 63, 63);
        border-left: 1px solid rgb(63, 63, 63);
        border-right: 1px solid rgb(63, 63, 63);
        color: white;
        font-family: 'Raleway', sans-serif;
    }

    ::v-deep .list-group-item:hover {
        border: none;
        outline: none;
        background-color: rgb(63, 63, 63);
        border-left: 1px solid rgb(63, 63, 63);
        border-right: 1px solid rgb(63, 63, 63);
        -webkit-box-shadow: none;
        box-shadow: none;
    }

</style>