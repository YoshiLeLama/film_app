<template>
  <div class="col no-select">
    <FilmDetailsPopup :id="id"
                      :movie-data="movieData"></FilmDetailsPopup>
    <div class="card shadow">
      <img alt="Banner" :src="movieData.poster" class="card-img-top" draggable="false"/>
      <div class="card-body">
        <h5 class="card-title">{{ movieData.title }}</h5>
        <p class="fs-6">{{ movieData.director }} · {{ movieData.release_date }}</p>
        <button @click="modal.show()"
           class="btn btn-primary position-relative">View more details
        </button>
      </div>
      <div class="card-footer text-muted">
        <p class="fs-6 m-0">Rating :
          <StarRating :max_stars="5" :rating="parseInt(movieData.rt_score)"></StarRating>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import FilmDetailsPopup from "@/components/FilmDetailsPopup";
import {Modal} from 'bootstrap'
import StarRating from "@/components/StarRating";

export default {
  name: "FilmPreview",
  components: {StarRating, FilmDetailsPopup},
  data() {
    return {
      showDetails: false,
      modal: null,
      activateFlip: true
    }
  },
  props: {
    id: String,
    movieData: Object
  },
  async mounted() {
    this.updateModal();
  },
  updated() {
    this.updateModal();
  },
  methods: {
    updateModal() {
      this.modal = new Modal(document.getElementById(this.id));
    }
  }
}
</script>

<style scoped>
.flip:hover {
  animation-name: flipY;
  animation-duration: 500ms;
  animation-iteration-count: 1;
  animation-timing-function: ease-out;
  animation-fill-mode: forwards;
  z-index: 1;
}

@keyframes flipY {
  from {
    transform: scale(1.0, 1.0);
  }
  to {
    transform: scale(1.05, 1.05);
  }
}
</style>