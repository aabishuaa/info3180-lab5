<template>
  <div>
    <h2>Add a Favorite Movie</h2>

    <div v-if="successMessage" class="alert alert-success">
      {{ successMessage }}
    </div>
    <ul v-if="errors.length" class="alert alert-danger">
      <li v-for="error in errors" :key="error">{{ error }}</li>
    </ul>

    <form
      id="movieForm"
      @submit.prevent="saveMovie"
      enctype="multipart/form-data"
    >
      <div class="form-group mb-3">
        <label for="title">Movie Title</label>
        <input type="text" name="title" class="form-control" />
      </div>

      <div class="form-group mb-3">
        <label for="description">Description</label>
        <textarea name="description" class="form-control"></textarea>
      </div>

      <div class="form-group mb-3">
        <label for="poster">Poster</label>
        <input type="file" name="poster" class="form-control" />
      </div>

      <button type="submit" class="btn btn-primary">Add Movie</button>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

let csrf_token = ref("");
let successMessage = ref("");
let errors = ref([]);

function getCsrfToken() {
  fetch("/api/v1/csrf-token")
    .then((response) => response.json())
    .then((data) => {
      csrf_token.value = data.csrf_token;
    });
}

function saveMovie() {
  let movieForm = document.getElementById("movieForm");
  let form_data = new FormData(movieForm);

  fetch("/api/v1/movies", {
    method: "POST",
    body: form_data,
    headers: {
      "X-CSRFToken": csrf_token.value,
    },
  })
    .then((response) => response.json())
    .then((data) => {
      if (data.message) {
        successMessage.value = data.message;
        errors.value = [];
        movieForm.reset();
      } else {
        successMessage.value = "";
        errors.value = data.errors;
      }
    })
    .catch((err) => {
      console.log(err);
    });
}

onMounted(() => {
  getCsrfToken();
});
</script>
