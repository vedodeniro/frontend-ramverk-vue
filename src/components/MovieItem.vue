<script setup>
import { ref } from "vue";

defineProps({
  movie: Object,
});

const posterFailed = ref(false);
</script>

<template>
  <li>
    <img
      v-if="movie.poster && !posterFailed"
      :src="movie.poster"
      :alt="movie.title"
      width="100"
      @error="posterFailed = true"
    />
    <div v-else>No poster</div>
    <p>{{ movie.title }}</p>
    <slot></slot>
  </li>
</template>
