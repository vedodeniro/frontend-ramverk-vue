<script setup>
defineProps({
  movie: Object,
});
</script>

<template>
  <li>
    <img v-if="movie.poster" :src="movie.poster" :alt="movie.title" width="100" />
    <div v-else>No poster</div>
    <p>{{ movie.title }}</p>
    <slot></slot>
  </li>
</template>
