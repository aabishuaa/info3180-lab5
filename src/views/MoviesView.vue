<template>
  <div>
    <h2>All Movies</h2>
    <div v-if="movies.length === 0">No movies yet.</div>

    <div class="movie-grid">
      <div class="movie-card" v-for="movie in movies" :key="movie.id">
        <img :src="movie.poster" alt="Poster" />
        <h4>{{ movie.title }}</h4>
        <p>{{ movie.description }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const movies = ref([]);

function fetchMovies() {
  fetch("/api/v1/movies")
    .then((response) => response.json())
    .then((data) => {
      movies.value = data.movies;
    });
}

onMounted(() => {
  fetchMovies();
});
</script>

<style scoped>
.movie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

.movie-card {
  border: 1px solid #ccc;
  padding: 1rem;
  border-radius: 10px;
  text-align: center;
}

.movie-card img {
  width: 100%;
  height: auto;
  max-height: 250px;
  object-fit: cover;
  margin-bottom: 10px;
}
</style>
