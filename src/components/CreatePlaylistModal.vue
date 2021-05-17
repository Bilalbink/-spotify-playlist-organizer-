<template>
    <div>
        <b-modal v-model="toggleModal" centered id="modal-scoped">
            <template #modal-header>
                <input v-model="name" class='playlist-name' placeholder="Playlist Name" />
            </template>
            <input v-model="description" class='playlist-desc' placeholder="Description" />
            <template #modal-footer>
                <b-button class="cancel-button" @click="closeModal" size="sm" variant="danger">
                    Cancel
                </b-button>
                <b-button class="save-button" @click="createPlaylist" size="sm" variant="success" >
                    Create
                </b-button>
        </template>
        </b-modal>
    </div>
</template>

<script>
export default {
    name: 'CreatePlaylistModal',
    props: {
        toggleModal: Boolean
    },
    data() {
        return {
            name: '',
            description: '',
        }
    },
    methods: {
        createPlaylist() {
            this.$emit('create-playlist', this.name, this.description);
            this.resetData();
        },
        closeModal() {
            this.$emit('toggle-modal');
            this.resetData();
        },
        resetData() {
            this.name = '';
            this.description = '';
        }
    }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400;700&display=swap');

.playlist-name {
    background-color: #181818;
    outline: none;
    border: none;
    font-family: 'Raleway', sans-serif;
    font-weight: 700;
    font-size: 25px;
    color: white;
    width: 100%;
}

.playlist-desc {
    background-color: #181818;
    outline: none;
    border: none;
    font-family: 'Raleway', sans-serif;
    font-size: 15px;
    color: white;
    margin-bottom: 75px;
    width: 100%;
}

.cancel-button {
     margin-left: 10px;
    margin-right: 10px;
    border-radius: 25px;
    min-width: 20%;
    height: 40px;
    outline: none;
    border: none;
    color: white;
    background-color: #E81123;
    font-family: 'Raleway', sans-serif;
    font-weight: 700;
}

.save-button {
    margin-left: 10px;
    margin-right: 10px;
    border-radius: 25px;
    min-width: 20%;
    height: 40px;
    outline: none;
    border: none;
    color: white;
    background-color: #17AA4A;
    font-family: 'Raleway', sans-serif;
    font-weight: 700;
}

::v-deep .modal-content {
    border: none;
    border-radius: 15px !important;
}

::v-deep .modal-header {
    color: white;
    background-color: #181818 !important;
    border-bottom: 1px solid #282828 !important; 
    font-family: 'Raleway', sans-serif;
}

::v-deep .modal-body {
    color: white;
    background-color: #181818;
    font-family: 'Raleway', sans-serif;
}

::v-deep .modal-footer {
    color: white;
    background-color: #181818;
    border-top: 1px solid #282828 !important; 
    font-family: 'Raleway', sans-serif;
}

</style>