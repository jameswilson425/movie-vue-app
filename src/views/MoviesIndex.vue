<template>
  <div class="movies-index">
    <div>
      <h1>All movies</h1>
      Search by name: <input v-model="titleFilter" list="titles" />
      <div>
        <button>Sort Alphabetically</button>
      </div>
      <datalist id="titles">
        <button v-on:click="setSortAttribute('title')" class="btn btn-success">
          Sort Alphabetically
        </button>
        <option
          v-for="movie in orderBy(
            filterBy(movies, titleFilter, 'title'),
            'title'
          )"
          >{{ movie.title }}</option
        >
      </datalist>
      <div v-for="movie in orderBy(movies, 'title')">
        <h2>{{ movie.title }}</h2>
        <p>Year: {{ movie.year }}</p>
        <p>Plot: {{ movie.plot }}</p>
        <router-link v-bind:to="`/movies/${movie.id}`">Movie Info</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      movies: [],
      titleFilter: "",
      sortAttribute: "title",
    };
  },
  created: function() {
    axios.get("/api/movies").then((response) => {
      console.log("movies index", response);
      this.movies = response.data;
    });
  },
  methods: {
    setSortAttribute: function(attribute) {
      this.sortAttribute = attribute;
    },
  },
};
</script>
