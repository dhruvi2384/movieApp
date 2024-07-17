<script lang="ts" setup>
import { reactive, ref } from "vue";
import AddNewMovie from "../components/modal/AddNewMovie.vue";
const rating = ref(1);
const isActiveModal = ref(false);
const isEditMode = ref(false);
const selectedMovie = ref(null);
const editMovieData = ref();
const movies = reactive([
  {
    id: 1,
    name: "The Godfather",
    description:
      "The aging patriarch of an organized crime dynasty transfers control of his clandestine empire to his reluctant son.",
    image:
      "https://m.media-amazon.com/images/M/MV5BM2MyNjYxNmUtYTAwNi00MTYxLWJmNWYtYzZlODY3ZTk3OTFlXkEyXkFqcGdeQXVyNzkwMjQ5NzM@._V1_FMjpg_UY1982_.jpg",
    rating: 4,
    genres: ["Crime", "Drama"],
    inTheaters: false,
  },
  {
    id: 2,
    name: "The Shawshank Redemption",
    description:
      "Two imprisoned men bond over a number of years, finding solace and eventual redemption through acts of common decency.",
    image:
      "https://m.media-amazon.com/images/M/MV5BNDE3ODcxYzMtY2YzZC00NmNlLWJiNDMtZDViZWM2MzIxZDYwXkEyXkFqcGdeQXVyNjAwNDUxODI@._V1_FMjpg_UX1200_.jpg",
    rating: 4,
    genres: ["Drama"],
    inTheaters: false,
  },
  {
    id: 3,
    name: "The Dark Knight",
    description:
      "When the menace known as the Joker wreaks havoc and chaos on the people of Gotham, Batman must accept one of the greatest psychological and physical tests of his ability to fight injustice.",
    image:
      "https://m.media-amazon.com/images/M/MV5BMTMxNTMwODM0NF5BMl5BanBnXkFtZTcwODAyMTk2Mw@@._V1_FMjpg_UY2048_.jpg",
    rating: 3,
    genres: ["Action", "Crime", "Drama"],
    inTheaters: false,
  },
]);

function dataAdd(item) {
  if (!item.id) {
    const userId = movies.length + 1;
    movies.push({
      id: userId,
      name: item.mname,
      description: item.mdes,
      rating: 1,
      genres: item.gSelect,
      image: item.mimage,
      inTheaters: false,
    });
    console.log(item);
  }

  if (item.id) {
    const index = movies.findIndex((data) => data.id == item.id);
    if (index !== -1) {
      movies[index].name = item.mname;
      movies[index].description = item.mdes;
      movies[index].genres = item.gSelect;
      movies[index].image = item.mimage;
    } else {
      console.log("Movie not found");
    }
  }
  isActiveModal.value = false;
  isEditMode.value = false;
  selectedMovie.value = null;
}
function removeCard(remove) {
  const index = movies.findIndex((data) => data.id == remove);

  if (index !== -1) {
    movies.splice(index, 1);
    console.log(movies, "updatedMovies");
  } else {
    console.log("Movie not found");
  }
}
function activeModal() {
  isActiveModal.value = true;
  editMovieData.value = null;
  console.log(movies);
}

function closeModal(item) {
  isActiveModal.value = item;
  if (!item) {
    isEditMode.value = false;
  }
}

function editModal(movie) {
  editMovieData.value = movie;
  isEditMode.value = true;
  isActiveModal.value = true;
}
</script>
<template>
  <div class="container">
    <v-btn @click="activeModal">Add Movie</v-btn>
    <ul class="movieList">
      <li v-for="movie in movies" :key="movie.id" class="mt-4">
        <div class="card">
          <div class="moviesec">
            <img :src="movie.image" :alt="movie.name" />
            <span class="star_rating">
              <span style="font-size: 500%" class="style_str"
                >&starf;
                <span class="ratinginnum">{{ movie.rating }}</span>
              </span>
            </span>
          </div>
          <h1>{{ movie.name }}</h1>
          <div>
            <span v-for="i in movie.genres" :key="i" class="tags">{{ i }}</span>
          </div>
          <p>{{ movie.description }}</p>
          <p>{{ movie.Released }}</p>
          <button v-for="ratings in 5" :key="ratings">
            <span
              @click="movie.rating = ratings"
              :class="ratings <= movie.rating ? 'yellow' : ''"
              style="font-size: 200%"
              >&starf;</span
            >
          </button>
        </div>
        <div>
          <v-btn @click="() => editModal(movie)">Edit</v-btn>
          <v-btn @click="removeCard(movie.id)">Delete</v-btn>
        </div>
      </li>
    </ul>
  </div>
  <AddNewMovie
    :title="isEditMode ? 'Edit Movie' : 'Add New Movie'"
    :activeModal="isActiveModal"
    :editMovieData="editMovieData"
    :isEditMovie="isEditMode"
    @closeModal="closeModal"
    @dataAdd="dataAdd"
  />
</template>
<style lang="css">
@import "../assets/style.css";
</style>
