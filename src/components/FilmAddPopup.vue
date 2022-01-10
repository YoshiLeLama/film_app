<template>
  <div class="modal fade">
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable w-100">
      <div class="modal-content">
        <div class="modal-header container" :class="{shadow: scrolled}">
          <h5 class="modal-title h-100 fw-bolder">Add Movie</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body" @scroll="scrolled = $event.target.scrollTop > 0">
          <div class="alert alert-success" role="alert" v-if="confirmSuccess">
            The movie has been successfully added.
          </div>
          <form id="addFilmForm" @submit.prevent="postNewFilm">
            <div class="mb-3">
              <label for="titleControlInput" class="form-label">Title</label>
              <input type="text" class="form-control" id="titleControlInput" name="title"
                     placeholder="Enter movie title" required>
            </div>
            <div class="mb-3">
              <label for="originalTitleControlInput" class="form-label">Original title</label>
              <input type="text" class="form-control" id="originalTitleControlInput" name="original_title"
                     placeholder="Enter movie original title" required>
            </div>
            <div class="mb-3">
              <label for="directorControlInput" class="form-label">Director</label>
              <input type="text" class="form-control" id="directorControlInput" name="director"
                     placeholder="Enter director's name" required>
            </div>
            <div class="mb-3">
              <label for="descriptionControlTextArea" class="form-label">Description</label>
              <textarea class="form-control" id="descriptionControlTextArea" rows="3"
                        placeholder="Enter movie description" name="description" required></textarea>
            </div>
            <div class="mb-3">
              <label for="imageControlInput" class="form-label">Movie poster</label>
              <input type="url" class="form-control" id="imageControlInput" name="poster" placeholder="Poster URL"
                     @input="changePreviewURL" required>
              <div class="accordion accordion-flush border border-top-0" id="imagePreviewAccordion" v-if="!isImageInputEmpty">
                <div class="accordion-item">
                  <h2 class="accordion-header" id="flush-headingOne">
                    <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#flush-collapseOne" aria-expanded="false" aria-controls="flush-collapseOne">
                      View image preview
                    </button>
                  </h2>
                  <div id="flush-collapseOne" class="accordion-collapse collapse" aria-labelledby="flush-headingOne" data-bs-parent="#imagePreviewAccordion">
                    <img :src="imagePreviewURL" v-if="imagePreviewURL.length > 0" class="img-fluid w-75" alt="Image Preview" draggable="false">
                    <p v-else class="p-2 m-0">Enter a valid url</p>
                  </div>
                </div>
              </div>
            </div>
            <div class="mb-3">
              <label for="releaseDateControlInput" class="form-label">Release Year</label>
              <input type="text" class="form-control" id="releaseDateControlInput" name="release_date"
                     placeholder="Enter release date" required>
            </div>
            <div class="mb-3">
              <label for="ratingControlInput" class="form-label">Rotten Tomatoes Score</label>
              <input type="text" class="form-control" id="ratingControlInput" name="rt_score"
                     placeholder="Enter Rotten Tomatoes Score" required>
            </div>
            <div class="mb-3">
              <button type="submit" class="btn btn-primary">Add Movie</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "FilmAddPopup",
  data() {
    return {
      scrolled: 0,
      confirmSuccess: false,
      imagePreviewURL: "",
      tooltip: Object,
      isImageInputEmpty: true
    }
  },
  mounted() {
  },
  updated() {
  },
  methods: {
    postNewFilm() {
      const formData = new FormData(document.getElementById("addFilmForm"))
      let newFilm = {}
      let valid = true
      formData.forEach((value, key) => {
        if (!value) valid = false;
        newFilm[key] = value
      })
      if (!valid) {
        alert("Missing fields");
        return
      }
      axios.post((process.env.VUE_APP_API_URL ?? "") + "/api/films/", JSON.stringify(newFilm)).then(
          () => {
            this.confirmSuccess = true
          }
      )
      document.getElementById("addFilmForm").reset()
      this.$emit("onPost")
    },
    isValidHttpUrl(string) {
      let url;
      try {
        url = new URL(string);
      } catch (_) {
        return false;
      }
      return url.protocol === "http:" || url.protocol === "https:";
    },
    changePreviewURL() {
      let url = document.getElementById('imageControlInput').value;
      this.isImageInputEmpty = url.length === 0
      if (this.isValidHttpUrl(url)) {
        axios.get(url).then(value => {
          this.imagePreviewURL = value.status === 404 ? "" : url;
        })
      }
    }
  },
  emits: [
    "onPost"
  ]
}
</script>

<style scoped>

</style>