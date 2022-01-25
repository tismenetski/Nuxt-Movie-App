<template>
<div class="genres container">
  <h1 class="genres-title">Please Select a Genre</h1>
  <NuxtLink :to="{name: 'index'}" class="button button-light">Back</NuxtLink>
  <ul class="genres-list" >
    <li v-for="(genre,index) in genres" :key="index" >
     <NuxtLink class="button button-light" :to="{name : 'movies-genresid', params : {genresid : genre.id , genre: genre.name}}">{{genre.name}}</NuxtLink>
    </li>
  </ul>
</div>
</template>

<script>
import axios from 'axios'
export default {
  name: "genres",
  data () {
    return {
    genres : []
    }
  },
  async fetch()  {
    await this.getGenres();
  },
  methods : {
    async getGenres() {
      const data = axios.get(`https://api.themoviedb.org/3/genre/movie/list?api_key=7de6e315194ae37cda48fe5d2273c6cf&language=en-US`)
      const response = await data;
      this.genres = response.data.genres;
      console.log(this.genres)
    }
  }
}
</script>

<style lang="scss" scoped>

.genres {

  .genres-title{
    text-align: center;
    color : #fff;
    font-size: 42px;
    padding: 40px 0;
  }

  .genres-list {
    padding: 40px 0;
    list-style: none;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-column-gap: 50px;
    grid-row-gap: 20px;

    li {
      padding: 30px 10px;
      background-color: #891b02;
      display: flex;
      justify-content: center;
      align-self: center;
      align-items: center;

    }
  }
}


@media (max-width: 800px) {
  .genres {

    .genres-title {
      font-size: 38px;
    }
    .genres-list {
      grid-template-columns: 1fr 1fr;
    }

  }

}

@media (max-width: 480px) {
  .genres {

    .genres-title {
      font-size: 30px;
    }
    .genres-list {
      grid-template-columns: 1fr 1fr;
      column-gap: 30px;

      li {
        padding: 0;
        background-color: transparent;

        a {
          width: 150px;
          text-align: center;
        }
      }
    }
  }

}

</style>
