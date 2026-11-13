<script setup>
defineProps({
  movie: Object,
});
</script>

<template>
  <li>
    {{ movie.title }}
    <slot></slot>
  </li>
</template>
