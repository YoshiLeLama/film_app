<template>
  <div class="col">
    <FilmDetailsPopup :id="id"
                      :data="movieData"></FilmDetailsPopup>
    <div class="card shadow">
      <img alt="Banner" :src="movieData.movie_banner" class="card-img-top"/>
      <div class="card-body">
        <h5 class="card-title">{{ movieData.title }}</h5>
        <p class="fs-6">{{ movieData.director }} · {{ movieData.release_date }}</p>
        <a @click="modal.show()"
           class="btn btn-primary position-relative">View more details
        </a>
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
      modal: null
    }
  },
  props: {
    id: String,
    movieData: Object
  },
  mounted() {
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
</style>