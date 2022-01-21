<template>
<div class="container single-genre">
<h1 class="single-genre-title">{{this.$route.params.genre}}</h1>
</div>
</template>

<script>
import axios from 'axios';
export default {
  name: "single-genre",
  data () {
    return {
      genreMovies  :[],
    }
  },
  async fetch() {
    await this.getSingleGenre();
  },
  methods : {
    async getSingleGenre() {
      console.log(this.$route.params.genresid);
      const data = axios.get(`https://api.themoviedb.org/3/discover/movie?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US&with_genres=${this.$route.params.genresid}`)
      const response = await data
      console.log(response)
      this.genreMovies = response.data.results;
      console.log(this.genreMovies)
    }
  }
}
</script>

<style lang="scss" scoped>

.single-genre {

  .single-genre-title {
    color : #fff;
    font-size: 43px;
    text-align: center;
    padding: 40px 0;
  }
}

</style>
