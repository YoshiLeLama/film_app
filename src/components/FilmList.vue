<template>
  <div class="w-75 mx-auto m-3">
    <FilmAddPopup id="add-film-popup" @onPost="getFilms"></FilmAddPopup>
    <div class="d-flex justify-content-end flex-wrap align-items-baseline">
      <button type="button" class="btn btn-primary me-3" id="refreshButton"
              @click="showAddMovieModal"
              @mouseenter="growPlusButton = true" @mouseleave="growPlusButton = false"><i class="bi bi-plus-circle"></i>
        Add movie
      </button>
      <button type="button" class="btn btn-outline-primary me-3 mt-1" @click="getFilms"
              @mouseenter="rotateRefresh = true" @mouseleave="rotateRefresh = false">
        <div :class="{rotate: rotateRefresh}"><i class="bi bi-arrow-clockwise"></i></div>
      </button>
    </div>
    <ListFilter :director_choices="directorChoices"
                :sorting_choices="sortingChoices"
                @titleFilterChange="titleFilter = $event"
                @directorFilterChange="directorFilter = $event"
                @sortingChange="updateSort($event)"></ListFilter>
    <div class="row justify-content-center row-cols-1 row-cols-sm-2 row-cols-xxl-6 row-cols-md-3 row-cols-xl-5 g-2">
      <FilmPreview
          v-for="film in films.filter(
              ({title, director}) => title.toLowerCase().search(titleFilter.toLowerCase()) !== -1
              && (directorFilter.length === 0 || director === directorFilter))"
          :id="film.id"
          :key="film.title"
          :movie-data="film"></FilmPreview>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import {Modal} from "bootstrap";
import FilmAddPopup from "@/components/FilmAddPopup";
import ListFilter from "@/components/ListFilter";
import FilmPreview from "@/components/FilmPreview";


const SortModes = {
  DIR_ASC: "Director A-Z",
  DIR_DESC: "Director Z-A",
  TITLE_ASC: "Title A-Z",
  TITLE_DESC: "Title Z-A",
  DATE_ASC: "Older",
  DATE_DESC: "Newer"
};

export default {
  name: "FilmList",
  components: {
    FilmAddPopup,
    ListFilter,
    FilmPreview
  },
  methods: {
    updateSort(mode) {
      if (mode === SortModes.DIR_ASC)
        this.films.sort(({director: A}, {director: B}) => A > B)
      else if (mode === SortModes.DIR_DESC)
        this.films.sort(({director: A}, {director: B}) => A < B)
      else if (mode === SortModes.TITLE_ASC)
        this.films.sort(({title: A}, {title: B}) => A > B)
      else if (mode === SortModes.TITLE_DESC)
        this.films.sort(({title: A}, {title: B}) => A < B)
      else if (mode === SortModes.DATE_ASC)
        this.films.sort(({release_date: A}, {release_date: B}) => A > B);
      else if (mode === SortModes.DATE_DESC)
        this.films.sort(({release_date: A}, {release_date: B}) => A < B);
    },
    updateModal() {
      this.movieAddModal = new Modal(document.getElementById("add-film-popup"));
    },
    showAddMovieModal() {
      console.log(this.movieAddModal.show())
    },
    getFilms() {
      this.films = []
      this.directorChoices = []
      let directors = {}
      axios.get((process.env.VUE_APP_API_URL ?? "") + "/api/films/", {}).then(
          async ({data}) => {
            for (let film of data) {
              if (!directors[film.director]) {
                directors[film.director] = (await axios.get(process.env.VUE_APP_API_URL + "/api/directors/" + film.director)).data.name
              }
              film.director = directors[film.director]
              this.films.push(film);
            }
            for (let director of Object.values(directors)) {
              this.directorChoices.push(director)
            }
          }
      ).catch(err => console.error('eee', err))
    }
  },
  data() {
    return {
      message: "",
      films: [],
      titleFilter: "",
      directorFilter: "",
      directorChoices: [],
      sortingChoices: Object.values(SortModes),
      movieAddModal: Modal,
      rotateRefresh: false,
      growPlusButton: false
    }
  },
  beforeMount() {
    this.getFilms();
  },
  mounted() {
    this.updateModal();
  },
  updated() {
    this.updateModal();
  }

}
</script>

<style scoped>
.rotate {
  animation: rotation 1s ease-out 1;
}

.growing {
  animation-name: grow;
  animation-duration: 1s;
  animation-iteration-count: 1;
}

@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@keyframes grow {
  0%, 100% {
    transform: scale(1, 1);
  }
  50% {
    transform: scale(1.2, 1.2);
  }
}
</style>