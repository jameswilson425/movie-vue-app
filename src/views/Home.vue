<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div>
      <h2>New Movie</h2>
      <ul>
        <li v-for="error in errors">{{ error }}</li>
      </ul>
      Title: <input type="text" v-model="newMovieTitle"><br>
      Year: <input type="text" v-model="newMovieYear"><br>
      Director: <input type="text" v-model="newMovieDirector"><br>
      Plot: <input type="text" v-model="newMoviePlot"><br>
      <button v-on:click="createMovie()">Create</button>
    </div>
  
   <div v-for="movie in movies">
      <h2>{{ movie.title }}</h2>
      <p>Year: {{ movie.year }}</p>
      <p>Director: {{ movie.director }}</p>
      <p>Plot: {{ movie.plot }}</p>
      <button v-on:click="showMovie(movie)">More Info</button>
    </div>

    <dialog id="movie-details">
      <form method="dialog">
        <h2>Movie Info</h2>
        {{ currentMovie }}
        <p>Title: <input type="text" v-model="currentMovie.title"></p>
        <p>Year: <input type="text" v-model="currentMovie.year"></p>
        <p>Director: <input type="text" v-model="currentMovie.director"></p>
        <p>Plot: <input type="text" v-model="currentMovie.plot"></p>
        <button v-on:click="updateMovie(currentMovie)">Update Movie</button>
        <button v-on:click="destroyMovie(currentMovie)">Destroy Movie</button>
        <button>Close</button>
      </form>
    </dialog>

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to the Greatest Movies Ever!",
      movies: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMovieDirector: "",
      newMoviePlot: "",
      currentMovie: {},
      errors: []
    };
  },
  created: function() {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/api/movies").then(response => {
        console.log(response.data);
        this.movies = response.data;
      });
    },
    createMovie: function () {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        director: this.newMovieDirector,
        plot: this.newMoviePlot
      };
      axios.post("/api/movies", params).then(response => {
        console.log("Success", response.data);
        this.movies.push(response.data);
        this.newMovieTitle = "";
        this.newMovieYear = "";
        this.newMovieDirector = "";
        this.newMoviePlot = "";
      })
      .catch(error => {
        console.log(error.response.data.errors);
        this.errors = error.response.data.errors;
      });
    },
    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function (movie) {
      var params = {
        title: movie.title,
        year: movie.year,
        director: movie.director,
        plot: movie.plot
      };
      axios.patch(`/api/movies/${movie.id}`, params).then(response => {
        console.log("Success", response.data);
      }).catch(error => {
        console.log(error.response.data.errors);
      });
    },
    destroyMovie: function (movie) {
      axios.delete(`/api/movies/${movie.id}`).then(response => {
        console.log("Success", response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    }
  }
};
</script>
