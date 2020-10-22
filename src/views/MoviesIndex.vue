<template>
  <div class="movies-index">
    <h1>All movies</h1>
    <div v-for="movie in movies">
      <h2>{{ movie.title }}</h2>
      <p>Year: {{ movie.year }}</p>
      <p>Plot: {{ movie.plot }}</p>
      <router-link v-bind:to="`/movies/${movie.id}`">Movie Info</router-link>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movies: [],
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      console.log("movies index", response);
      this.movies = response.data;
    });
  },
  methods: {},
};
</script>
