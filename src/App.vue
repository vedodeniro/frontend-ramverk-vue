<script setup>
import { ref, computed } from "vue";

const searchMovie = ref("");

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
</script>

<template>
  <main>
    <h1>Movie Watchlist</h1>

    <input v-model="searchMovie" type="text" placeholder="Search for a movie" />

    <ul>
      <li v-for="movie in searchResults" :key="movie.id">{{ movie.title }}</li>
    </ul>

    <p v-if="searchResults.length === 0">No movies found!</p>
  </main>
</template>
