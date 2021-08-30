<template>
  <div class="app">
    <div class="menu">
      <SideBar
        :allGenres="allGenres"
        @name="getSingleGenre"
        @search="callGetGenreMovies()"
      />
    </div>
    <div class="movies">
      <Movies :allMovies="allMovies" :loading="loading" :errors="errors" />
      <div v-if="showLoadMore" class="load-btn">
        <button @click="loadMore()">Load More</button>
      </div>
    </div>
  </div>
</template>

<script>
import SideBar from "./components/SideBar.vue";
import Movies from "./components/Movies.vue";

import axios from "axios";

export default {
  name: "App",
  components: {
    SideBar,
    Movies,
  },
  data() {
    return {
      baseUrl: "https://api.themoviedb.org/3",
      imgBaseUrl: "https://image.tmdb.org/t/p/w500",
      apiKey: process.env.VUE_APP_API_KEY,
      lang: "language=en-US",
      page: 1,
      allMovies: [],
      allGenres: [],
      selectedGenre: [],
      loading: true,
      errors: "",
      showLoadMore: true,
    };
  },

  mounted() {
    this.fetchAllMovies(this.page);
    this.fetchAllGenres();
  },
  methods: {
    async fetchAllMovies(page) {
      this.selectedGenre = [];
      const data = await axios.get(
        `${this.baseUrl}/discover/movie?sort_by=popularity.desc&${this.apiKey}&${this.lang}&page=${page}`
      );

      await data.data.results.forEach((ele) => this.allMovies.push(ele));

      this.loading = false;
    },
    loadMore() {
      this.page += 1;
      this.selectedGenre.length > 0
        ? this.getGenreMovies(this.page)
        : this.fetchAllMovies(this.page);
    },

    async fetchAllGenres() {
      const data = await axios.get(
        `${this.baseUrl}/genre/movie/list?${this.apiKey}&${this.lang}`
      );
      this.showLoadMore = true;
      this.errors = "";
      this.allGenres = await data.data.genres;
      this.loading = false;
    },
    getSingleGenre(genre) {
      this.page = 1;

      if (!this.selectedGenre.includes(genre)) {
        this.selectedGenre.push(genre);
      } else {
        this.selectedGenre.splice(this.selectedGenre.indexOf(genre), 1);
      }
    },
    async getGenreMovies(page) {
      this.loading = true;

      const data = await axios.get(
        `${this.baseUrl}/discover/movie?${this.apiKey}&${this.lang}&with_genres=${this.selectedGenre}&page=${page}`
      );
      if (data.data.results.length > 0) {
        await data.data.results.forEach((ele) => this.allMovies.push(ele));
        this.showLoadMore = true;
        this.errors = "";
      } else {
        this.errors = "No items were found that match your query.";
        this.showLoadMore = false;
      }
      this.loading = false;
    },

    callGetGenreMovies() {
      this.allMovies = [];
      this.page = 1;
      this.getGenreMovies(this.page);
    },
  },
};
</script>

<style lang="scss">
@import "@/assets/_shared.scss";
@import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@700&display=swap");

* {
  padding: 0;
  margin: 0;
  box-sizing: 0;
}
body {
  width: 100%;
  color: #2c3e50;
}
.app {
  @include bigFlexCenter;
  width: 90%;
  margin: 3rem auto;
  font-family: Source Sans Pro, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
h2 {
  margin-bottom: 2rem;
}
.menu {
  width: 18rem;
}
.movies {
  width: calc(100% - 19rem);
}
.load-btn {
  margin: 1.2rem 0 0 1rem;
  background: $bg-btn-color;
  border-radius: $border-radius;
  padding: 0.8rem 0.9rem;
  text-align: center;

  & button {
    cursor: pointer;
    background: none;
    border: none;
    font-size: 1.4rem;
    color: white;
    font-weight: bold;

    &:hover {
      color: black;
    }
  }
}
</style>
