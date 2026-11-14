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

<style>
body {
  margin: 0;
  background: #0a1a3a;
  color: #e8edf7;
  font-family: sans-serif;
}

main {
  max-width: 1100px;
  margin: 0 auto;
  padding: 24px 16px;
}

h1 {
  text-align: center;
  color: #2dd4bf;
  font-size: 40px;
  margin-bottom: 24px;
}

h2 {
  color: #2dd4bf;
  margin-top: 32px;
}

input {
  padding: 10px 14px;
  width: 260px;
  margin-right: 12px;
  background: #16295a;
  color: #ffffff;
  border: 2px solid #2dd4bf;
  border-radius: 8px;
  font-size: 16px;
}

input::placeholder {
  color: #8fa3cf;
}

input:focus {
  outline: none;
  border-color: #ffffff;
}

button {
  padding: 8px 12px;
  background: #2dd4bf;
  color: #000000;
  border: none;
  border-radius: 6px;
  font-size: 13px;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover {
  background: #7ee8da;
}

ul {
  list-style: none;
  padding: 0 0 12px 0;
  display: flex;
  gap: 16px;
  overflow-x: auto;
  scrollbar-color: #2dd4bf #1c3166;
}

li {
  flex: 0 0 140px;
  box-sizing: border-box;
  text-align: center;
  background: #e8edf7;
  color: #000000;
  border-radius: 10px;
  padding: 10px;
}

li img {
  display: block;
  width: 100%;
  aspect-ratio: 380 / 562;
  object-fit: cover;
  border-radius: 8px;
}

li div {
  width: 100%;
  aspect-ratio: 380 / 562;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #c9d3ea;
  border-radius: 8px;
}

li p {
  height: 42px;
  margin: 10px 0;
  overflow: hidden;
}

li button {
  margin: 4px 2px;
}
</style>
