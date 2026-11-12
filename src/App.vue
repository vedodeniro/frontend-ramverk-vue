<script setup>
import { ref } from "vue";

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
</script>

<template>
  <main>
    <h1>Movie Watchlist</h1>
    <ul>
      <li v-for="movie in movies" :key="movie.id">{{ movie.title }}</li>
    </ul>
  </main>
</template>
