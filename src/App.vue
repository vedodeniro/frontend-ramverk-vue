<script setup>
import { ref, computed } from "vue";
import MovieItem from "./components/MovieItem.vue";

const searchMovie = ref("");
const watchlist = ref([]);
const watched = ref([]);

const movies = ref([
  { id: 1, title: "Fight Club" },
  { id: 2, title: "Forrest Gump" },
  { id: 3, title: "Inception" },
  { id: 4, title: "Interstellar" },
  { id: 5, title: "Gladiator" },
  { id: 6, title: "Pulp Fiction" },
  { id: 7, title: "The Dark Knight" },
  { id: 8, title: "The Godfather" },
  { id: 9, title: "The Matrix" },
  { id: 10, title: "Jurassic Park" },
]);

const searchResults = computed(() => {
  return movies.value.filter((movie) =>
    movie.title.toLowerCase().includes(searchMovie.value.toLowerCase()),
  );
});

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

    <input v-model="searchMovie" type="text" placeholder="Search for a movie" />
    <ul>
      <MovieItem v-for="movie in searchResults" :key="movie.id" :movie="movie">
        <button @click="addToWatchlist(movie)">Add to watchlist</button>
        <button @click="markAsWatched(movie)">Mark as watched</button>
      </MovieItem>
    </ul>

    <p v-if="searchResults.length === 0">No movies found!</p>

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
