<template>
  <div class="home">
    <p><input type="text" v-model="search"> <button>search</button></p>
    <button class="btn">Show Movies Only</button>
    <button class="btn">Show Games Only</button>
    <div v-for="(movie, i) in displayItems" :key="i">
      <hr>
        <hr>
        <small>{{ movie.theme }}</small>
        <h2>{{ movie.context }}</h2>
        <p>Source: {{ movie.source }}</p>
        <p>Quote: {{ movie.quote }}</p>
    </div>
    <hr>
    <hr>
    <p>Showing {{ displayItems.length }} of {{ moviesLength }}</p>
    <div>
      <button
        v-if="currentPage != 1"
        class="btn"
        @click="prevPage()"
      >
        Show prev 15
      </button>
      <button
        v-if="currentPage != fifteenMovies.length"
        @click="nextPage()"
        class="btn"
      >
        Show next 15
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios"
  export default {
    data() {
      return {
        movies: [],
        displayItems: [],
        filteredMovies: [],
        fifteenMovies: [],
        reindexedMovies: [],
        pages: [],
        currentPage: 1,
        moviesLength: 0,
        search: "",
      }
    },
    created() {
      this.indexMovies();
    },
    methods: {
      indexMovies() {
        axios.get("https://gist.githubusercontent.com/benchprep/dffc3bffa9704626aa8832a3b4de5b27/raw/quotes.json")
        .then((res)=> {
          this.movies = res.data;
          this.moviesLength = res.data.length;
          var i = 0;
          var pages = 1;
          var fifteenMovies = [];
          while (i < res.data.length) {
            fifteenMovies.push(res.data[i]);
            if (fifteenMovies.length == 15) {
              this.fifteenMovies.push(fifteenMovies);
              fifteenMovies = [];
              this.pages.push(pages ++)
            }
            i ++;
          }
          this.fifteenMovies.push(fifteenMovies);
          this.pages.push(pages ++);
          this.displayItems = this.fifteenMovies[0];
        })
      },
      nextPage() {
        this.currentPage++;
        this.displayItems = this.fifteenMovies[this.currentPage - 1];
      },
      prevPage() {
        this.currentPage--;
        this.displayItems = this.fifteenMovies[this.currentPage - 1];
      },
    },
  }
</script>

<style>
  .btn {
    display: inline-block;
    margin: 0 10px;
  }
</style>