<template>
  <Loading v-if="$fetchState.pending" />
<div v-else class="container single-movie">
<NuxtLink :to="{name : 'index'}" class="button">Back</NuxtLink>
  <div class="movie-info">
    <div class="movie-img">
      <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
    </div>
    <div class="movie-content">
      <h1>Title: {{movie.title}}</h1>
      <p class="movie-fact tagline">
        <span>Tagline:</span> "{{movie.tagline}}"
      </p>
      <p class="movie-fact">
<span>Released:</span>
         {{
        new Date(movie.release_date).toLocaleString('en-us', {
        month: 'long',
        day : 'numeric',
        year: 'numeric'})
        }}
      </p>
      <p class="movie-fact">
        <span>Duration:</span> {{movie.runtime}} minutes
      </p>
      <p class="movie-fact">
        <span>Budget:</span> {{movie.budget.toLocaleString('en-us', {style : 'currency', currency : 'USD'})}}
      </p>
      <p class="movie-fact">
        <span>Revenue:</span>
        {{movie.revenue.toLocaleString('en-us', {style : 'currency', currency : 'USD'})}}
      </p>
      <p class="movie-fact">
        <span>Overview:</span> {{movie.overview}}
      </p>
      <p class="movie-fact">
        <span>Vote Average:</span> {{ movie.vote_average}}
      </p>
    </div>
  </div>
  
  <h2 class="trailer-header">Watch Trailer</h2>
  <no-ssr  placeholder="Loading...">
<!--    <youtube :player-vars="{ autoplay: 1 }"  :player-width="100" :player-height="100"  :video-id="PnJY20UCH9c"    />-->
    <youtube class="youtube-video" player-width="100%" player-height="400"  :video-id="trailerVideo.key" ></youtube>
  </no-ssr>
<!--  <youtube :video-id="aC40LnWR9TE"></youtube>-->
</div>
</template>

<script>
// This is a route param component
import axios from 'axios';

export default {
  name: "single-movie",
  data() {
    return {
      movie : '',
      images : [],
      videos : [],
      trailerVideo : {
        key : '',
      },
      window : {
        height: '',
        width : ''
      }

    }
  },
  async fetch() {
    await this.getSingleMovie();
    await this.getSingleMovieImages();
    await this.getSingleMovieVideos();
  },
  head() {
    return {
      title : this.movie.title,
    }
  },

  fetchDelay : 1000,

  // using this we can have a listener on the window size
  beforeMount() {
    window.addEventListener('resize', this.handleResize);
    this.handleResize();
  },

  beforeDestroy() {
    window.removeEventListener('resize', this.handleResize);
  },

  methods : {

    // Get the movie information object
    async getSingleMovie() {
      const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US`)
      const result = await data;
      this.movie = result.data;
    },

    // get movie images
    async getSingleMovieImages() {
      const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/images?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US&include_image_language=en`)
      const result = await data;
      this.images = result.data;
    },

    // Get the trailer for a movie
    async getSingleMovieVideos() {
      const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/videos?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US&include_image_language=en`)
      const result = await data;
      this.videos = result.data;

      this.videos.results.forEach(video => {

        if (video.type === 'Trailer') {
          this.trailerVideo.key = video.key;
        }
      });
    },

    handleResize() {
      this.window.width = window.innerWidth;
      this.window.height = window.innerHeight;
    },

  },



}
</script>

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}

.trailer-header {
  margin-top: 50px;
  text-align: center;
}

.youtube-video {
  margin-top: 50px;
}

</style>
