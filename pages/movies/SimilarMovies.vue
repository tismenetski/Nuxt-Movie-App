<template>
<div class="container">
  <h1 class="title">
    Similar Movies
  </h1>
  <div class="similar-movies">
    <ul  class="single-genre-list">
      <li v-for="(movie,index) in movies" :key="index">
        <div class="single-movie">
          <div class="single-movie-image"><img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt=""></div>
          <div class="single-movie-info">
            <p>Movie Name: {{movie.title}}</p>
            <p>Overview: {{movie.overview}}</p>
            <p>Release Date: {{movie.release_date}}</p>
            <p>Vote Average: {{movie.vote_average}}</p>
          </div>
        </div>
      </li>
    </ul>
  </div>
</div>
</template>

<script>
import axios from "axios";

export default {
  name: "SimilarMovies",
  data () {
    return {
      movies : []
    }
  },
  async fetch() {
    await this.getSimilarMovies();
  },
  methods : {
    async getSimilarMovies(){
      const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/similar?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US`)
      const response = await data
      this.movies = response.data.results;
    }
  }
}
</script>

<style lang="scss" scoped>


.title {
  color : #fff;
  font-size: 43px;
  text-align: center;
  padding: 40px 0;
}



  .button {
    margin-bottom: 30px;
  }

  .single-genre-title {
    color : #fff;
    font-size: 43px;
    text-align: center;
    padding: 40px 0;
  }

  .single-genre-list {
    list-style: none;
    color: #fff;
  }

  .single-movie {
    display: flex;
    flex-direction: row;

  }

  .single-movie-info {

    p {
      margin-left: 20px;
      padding: 20px 0;
      font-size: 25px;
    }
    .button{
      margin-left: 20px;
    }
  }

  .filter-items {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    list-style: none;
    gap: 50px;

    .button-filter {
      padding: 10px 100px;
    }

    .button-filter-active {
      background-color: #000;
    }
  }

@media (max-width: 480px) {



    .single-movie {
      flex-direction: column;

      .single-movie-image img {
        max-width:100%;
        max-height:100%;

      }
    }

    .filter-items {
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }
  }




</style>
