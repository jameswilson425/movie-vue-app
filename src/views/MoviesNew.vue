<template>
  <div class="movies-new">
    <h1>New Movie</h1>
    <form v-on:submit.prevent="createMovie()">
      <ul>
        <li v-for="error in errors">{{ error }}</li>
      </ul>
      Title: <input type="text" v-model="newMovieTitle" />
      Year: <input type="text" v-model="newMovieYear" />
      <div>
      Plot: <input type="text" class="form-control" v-model="newMoviePlot">
      <small>{{ 100 - newMoviePlot.length }} characters remaining</small> 
      <input type="submit" value="Create" />
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      errors: [],
    };
  },
  created: function() {},
  methods: {
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          console.log("movies create", response);
          this.$router.push("/movies");
        })
        .catch(error => {
          console.log("movies create error", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>