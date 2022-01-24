<template>
<div class="container single-genre">
<h1 class="single-genre-title">{{$route.params.genre}}</h1>
  <NuxtLink class="button" :to="{name : 'movies-genres'}">Back</NuxtLink>
  <div class="genre-filter">
    <ul class="filter-items">
      <li>
        <button class="button button-filter" :class="{'button-filter-active':genreFilters.highestRated.active}" @click="activateFilter" >
            Top Rated
        </button>
      </li>
      <li>
        <button class="button button-filter" :class="{'button-filter-active':genreFilters.upcoming.active}"  @click="activateFilter"  >
          Upcoming
        </button>
      </li>
    </ul>
  </div>
  <ul v-if="!filterActive" class="single-genre-list">
    <li v-for="(movie,index) in genreMovies" :key="index">
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
  <ul v-else class="single-genre-list">
    <li v-for="(movie,index) in filteredMovies" :key="index">
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
</template>

<script>
import axios from 'axios';
export default {
  name: "single-genre",
  data () {
    return {
      genreMovies  :[],
      filterActive : false,
      filteredMovies : [],
      filterName : '',
      genreFilters : {
        highestRated : {
          active : false,
          movies : []
        },
        upcoming : {
          active : false,
          movies : []
        },
        nowPlaying : {
          active : false,
          movies : []
        },

      }
    }
  },
  async fetch() {
    await this.getSingleGenre();
    await this.getTopRatedByGenre();
    await this.getUpcomingByGenre();
  },
  methods : {
    async getSingleGenre() {
      const data = axios.get(`https://api.themoviedb.org/3/discover/movie?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US&with_genres=${this.$route.params.genresid}`)
      const response = await data
      this.genreMovies = response.data.results;
    },
    async getTopRatedByGenre() {
      const data = axios.get(`https://api.themoviedb.org/3/movie/top_rated?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US&with_genres=${this.$route.params.genresid}`)
      const response = await data
      this.genreFilters.highestRated.movies = response.data.results;
    },
    async getUpcomingByGenre() {
      const data = axios.get(`https://api.themoviedb.org/3/movie/upcoming?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US&with_genres=${this.$route.params.genresid}`)
      const response = await data
      this.genreFilters.upcoming.movies = response.data.results;
    },
    activateFilter(event) {
      // Clean all existing filters
      for (const [key,value] of Object.entries(this.genreFilters)) {
         value.active = false;
        console.log(value);
        console.log(key);
      }
      this.filteredMovies = [];

      // Get the text from the button
      const buttonText = event.target.innerText;

      // if we currently have a filter set and the same filter button clicked, we disable the filter
      if (this.filterActive && this.filterName === buttonText) {
        this.filterActive = false;
        this.filterName = '';
        return;
      }
      switch (buttonText) {
        case 'Top Rated':
            this.genreFilters.highestRated.active = true;
            this.filteredMovies = this.genreFilters.highestRated.movies;
            this.filterName = buttonText;
          break;
          case 'Upcoming':
              this.genreFilters.upcoming.active = true;
              this.filteredMovies = this.genreFilters.upcoming.movies;
              this.filterName = buttonText;
            break;
          default:
            break;
      }
      this.filterActive = true;

    },
  }
}
</script>

<style lang="scss" scoped>

.single-genre {

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
      padding: 20px;
      font-size: 25px;
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
}

</style>
