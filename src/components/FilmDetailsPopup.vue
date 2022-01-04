<template>
  <div class="modal fade">
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable w-100">
      <div class="modal-content">
        <div class="modal-header container">
          <h5 class="modal-title h-100 fw-bolder">{{ data.title }}
            <span class="badge rounded-pill bg-secondary align-baseline">{{ data.release_date }}</span></h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <p class="fs-6 text-start">{{ data.original_title }}</p>
          <StarRating class="position-absolute top-0 end-0 m-3" :rating="parseInt(data.rt_score)" :max_stars="5"></StarRating>
          <img alt="Film image" v-bind:src="data.image" class="w-100 rounded"/>
          <p class="fs-4 mt-3 fw-bold text-start">{{ data.director }}</p>
          <p class="fs-6 m-4">{{ data.description }}</p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import StarRating from "@/components/StarRating";

export default {
  name: "FilmDetailsPopup",
  components: {StarRating},
  props: {
    data: Object
  },
  computed: {
    toHours(inMinutes) {
      let hours = Math.floor(inMinutes / 60);
      let minutes = inMinutes % 60;
      return hours + 'h' + minutes;
    }
  }
}
</script>

<style scoped>
.modal-title * {
}
</style>