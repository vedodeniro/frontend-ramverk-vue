<script setup>
import { ref } from "vue";
import MovieItem from "./components/MovieItem.vue";

const apiKey = import.meta.env.VITE_OMDB_KEY;

const searchMovie = ref("");
const searchResults = ref([]);
const hasSearched = ref(false);
const watchlist = ref([]);
const watched = ref([]);

async function searchMovies() {
  if (searchMovie.value.trim() === "") {
    return;
  }

  const response = await fetch(
    `https://www.omdbapi.com/?s=${encodeURIComponent(searchMovie.value)}&apikey=${apiKey}`,
  );
  const data = await response.json();
  hasSearched.value = true;

  if (data.Response === "True") {
    searchResults.value = data.Search.map((item) => ({
      id: item.imdbID,
      title: item.Title,
      poster: item.Poster === "N/A" ? "" : item.Poster,
    }));
  } else {
    searchResults.value = [];
  }
}

function addToWatchlist(movie) {
  const alreadyInList = watchlist.value.some((item) => item.id === movie.id);
  if (!alreadyInList) {
    watchlist.value.push(movie);
  }
}

function markAsWatched(movie) {
  watchlist.value = watchlist.value.filter((item) => item.id !== movie.id);
  const alreadyWatched = watched.value.some((item) => item.id === movie.id);
  if (!alreadyWatched) {
    watched.value.push(movie);
  }
}

function removeFromWatchlist(movie) {
  watchlist.value = watchlist.value.filter((item) => item.id !== movie.id);
}

function removeFromWatched(movie) {
  watched.value = watched.value.filter((item) => item.id !== movie.id);
}
</script>

<template>
  <main>
    <h1>Movie Watchlist</h1>

    <input
      v-model="searchMovie"
      type="text"
      placeholder="Search for a movie"
      @keyup.enter="searchMovies"
    />
    <button @click="searchMovies">Search</button>

    <ul>
      <MovieItem v-for="movie in searchResults" :key="movie.id" :movie="movie">
        <button @click="addToWatchlist(movie)">Add to watchlist</button>
        <button @click="markAsWatched(movie)">Mark as watched</button>
      </MovieItem>
    </ul>

    <p v-if="hasSearched && searchResults.length === 0">No movies found!</p>

    <h2>Watchlist</h2>
    <ul>
      <MovieItem v-for="movie in watchlist" :key="movie.id" :movie="movie">
        <button @click="markAsWatched(movie)">Mark as watched</button>
        <button @click="removeFromWatchlist(movie)">Remove</button>
      </MovieItem>
    </ul>
    <p v-if="watchlist.length === 0">Your watchlist is empty!</p>

    <h2>Watched</h2>
    <ul>
      <MovieItem v-for="movie in watched" :key="movie.id" :movie="movie">
        <button @click="removeFromWatched(movie)">Remove</button>
      </MovieItem>
    </ul>
    <p v-if="watched.length === 0">Your watchedlist is empty!</p>
  </main>
</template>
