<template>
  <div class="modal fade">
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable w-100 w-75">
      <div class="modal-content">
        <div class="modal-header container" :class="{shadow: scrolled}">
          <h5 class="modal-title h-100 fw-bolder">{{ movieData.title }}
            <span class="badge rounded-pill bg-secondary align-baseline">{{ movieData.release_date }}</span></h5>
        </div>
        <div class="modal-body" id="popupBody" @scroll="scrolled = $event.target.scrollTop > 0">
          <p class="fs-6 text-start w-75">{{ movieData.original_title }}</p>
          <StarRating class="position-absolute top-0 end-0 m-3" :rating="parseInt(movieData.rt_score)"
                      :max_stars="5"></StarRating>
          <img alt="Film image" :src="movieData.poster" class="w-100 rounded" @dragstart.prevent/>
          <p class="fs-4 mt-3 fw-bold text-start">{{ movieData.director }}</p>
          <p class="fs-6 m-4">{{ movieData.description }}</p>
          <div class="alert alert-success" role="alert" v-if="showCopySuccess">
            Data has been copied to clipboard.
          </div>
          <div class="d-flex align-content-start">
            <button type="button" class="btn btn-outline-secondary btn-sm" id="copyButton" @click="dataToClip" data-toggle="tooltip" data-bs-placement="right" title="Copy data to clipboard">
              <i class="bi bi-clipboard"></i>
            </button>
          </div>
        </div>
        <div class="modal-footer justify-content-center">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="scrolled = false">
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import StarRating from "@/components/StarRating";
import {Tooltip} from 'bootstrap';

export default {
  name: "FilmDetailsPopup",
  components: {StarRating},
  data() {
    return {
      scrolled: false,
      showCopySuccess: false,
      copyButtonTooltip: Tooltip,
      copyTimeout: null
    }
  },
  props: {
    movieData: Object
  },
  mounted() {
    this.copyButtonTooltip = new Tooltip(document.getElementById('copyButton'))
  },
  updated() {
    this.copyButtonTooltip = new Tooltip(document.getElementById('copyButton'))
  },
  methods: {
    dataToClip() {
      clearTimeout(this.copyTimeout)
      this.copyButtonTooltip.hide()
      navigator.clipboard.writeText(JSON.stringify(this.movieData, null, "\t"))
      this.showCopySuccess = true
      this.copyTimeout = setTimeout(() => this.showCopySuccess = false, 5000)
    }
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
</style>