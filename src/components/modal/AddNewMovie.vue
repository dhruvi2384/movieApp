<script setup lang="ts">
import { ref, watch } from "vue";

// Props
const props = defineProps({
  title: String,
  activeModal: Boolean,
  editMovieData: Object,
});

// Emit
const emit = defineEmits(["dataAdd", "closeModal"]);

// Data
const movieName = ref("");
const description = ref("");
const movieImage = ref("");
const checkBox = ref("");
const optionGeners = ref(["comedy", "good", "bed"]);
const selectGenera = ref([]);
const isRquired = ref(false);
const isdesRequired = ref(false);
const isRequiredPath = ref(false);
const isEdit = ref(false);
const movieId = ref(null);
// use emit in create button
console.log("editMovieData: ", props.editMovieData);
watch(
  () => props.editMovieData,
  (newValue) => {
    if (newValue) {
      movieId.value = newValue.id;
      movieName.value = newValue.name;
      description.value = newValue.description;
      movieImage.value = newValue.image;
      selectGenera.value = newValue.genres;
    }
  }
);

function addData() {
  var item = {
    id: movieId.value,
    mname: movieName.value,
    mdes: description.value,
    mimage: movieImage.value,
    incinema: checkBox.value,
  };
  console.log(item, "id");
  if (item.mname == "" && item.mdes == "" && item.mimage == "") {
    isRquired.value = true;
    isdesRequired.value = true;
    isRequiredPath.value = true;
  } else {
    emit("dataAdd", item);
    (movieId.value = null),
      (movieName.value = null),
      (description.value = null),
      (movieImage.value = null);
  }
}

// movie name input
function movieChange(item) {
  if (movieName.value) {
    isRquired.value = false;
  }
  if (!movieName.value) {
    isRquired.value = true;
  }
}

// movie description
function chnageDes(item) {
  if (description.value) {
    isdesRequired.value = false;
  }
  if (!description.value) {
    isdesRequired.value = true;
  }
}

// movie path
function moviePathChnages(item) {
  if (movieImage.value) {
    isRequiredPath.value = false;
  }
  if (!movieImage.value) {
    isRequiredPath.value = true;
  }
}
</script>
<template>
  <v-dialog max-width="500" v-model="props.activeModal">
    <v-card :title="props.title">
      <v-card-text>
        <form>
          <ul class="modal_data">
            <li>
              <label for="Name">Enter Movie Name</label>
              <input
                type="text"
                id="Name"
                @input="movieChange"
                v-model="movieName"
                required
              />
              <p class="text-error" v-if="isRquired == true">is required</p>
            </li>
            <li>
              <label for="dName">Description</label>
              <textarea
                type="text"
                id="dName"
                @input="chnageDes"
                v-model="description"
                required
              />
              <p class="text-error" v-if="isdesRequired == true">is required</p>
            </li>
            <li>
              <label for="cars">Genres</label>
              <select name="cars" id="cars" v-model="selectGenera" multiple>
                <option
                  v-for="options in optionGeners"
                  :key="options"
                  :value="options"
                >
                  {{ options }}
                </option>
              </select>
            </li>
            <li>
              <label for="imgpath">Image Path</label>
              <textarea
                type="text"
                id="imgpath"
                v-model="movieImage"
                @input="moviePathChnages"
                required
              />
              <p class="text-error" v-if="isRequiredPath == true">
                is required
              </p>
            </li>
            <li>
              <div class="checkbox">
                <input
                  type="checkbox"
                  v-model="checkBox"
                  id="inTheaters"
                  required
                />
                <label for="inTheaters">In Theaters</label>
              </div>
            </li>
          </ul>
        </form>
      </v-card-text>

      <v-card-actions>
        <div class="footer">
          <v-btn text="Close" @click="emit('closeModal', false)"></v-btn>
          <v-btn @click="addData">{{
            props.title == "Edit Movie" ? "Update" : "Create"
          }}</v-btn>
        </div>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
