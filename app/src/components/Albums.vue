<template>
  <div class="albums">
    <h2>Albums</h2>
    <ul v-if="albums">
      <li
        v-for="album in albums"
        :key="album.id"
      >
        <router-link :to="`/albums/${album.id}`">
          {{ album.title }} ({{ album.imageCount }} image{{ album.imageCount !== 1 ? 's' : '' }})
        </router-link>
      </li>
    </ul>
    <AlbumForm
      :onChange="handleAdd"
    />
  </div>
</template>

<script>
import AlbumForm from './AlbumForm';
import { getAlbums, addAlbum } from '../services/api';

export default {
  data() {
    return {
      albums: null
    };
  },
  props: ['albumstats'],
  components: {
    AlbumForm
  },
  created() {
    getAlbums()
      .then(albumlist => {
        this.albums = albumlist;
      })
      .catch(err => {
        this.error = err;
      });
  },
  methods: {
    handleAdd(album) {
      return addAlbum(album)
        .then(saved => {
          this.albums.push(saved);
          this.$router.push(`/albums/${saved.id}`);
        })
        .catch(err => {
          this.error = err;
        });
    }
  }
};

</script>

<style scoped>
.albums {
  text-align: center;
}
li {
  border: rgb(179, 179, 179);
  border-style: solid;
  border-width: .5px;
  border-radius: 3px;
  text-align: left;
  margin: 5px;
  margin-left: 37%;
  width: 300px;
  list-style: none;
}
li:hover {
  cursor: pointer;
  background-color: rgba(176, 196, 222, 0.795);
}
</style>
