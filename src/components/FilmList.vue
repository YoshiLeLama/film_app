<template>
  <div class="w-75 mx-auto m-3">
    <ListFilter :director_choices="directorChoices"
                :sorting_choices="sortingChoices"
                @titleFilterChange="titleFilter = $event"
                @directorFilterChange="directorFilter = $event"
                @sortingChange="updateSort($event)"></ListFilter>
    <div class="row justify-content-center row-cols-1 row-cols-md-5 g-2">
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
import FilmPreview from "@/components/FilmPreview";
import ListFilter from "@/components/ListFilter";

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
    }
  },
  data() {
    return {
      message: "",
      films: [],
      titleFilter: "",
      directorFilter: "",
      directorChoices: [],
      sortingChoices: Object.values(SortModes)
    }
  },
  beforeMount() {
    axios.get("https://ghibliapi.herokuapp.com/films").then(
        movies => {
          for (let data of movies.data) {
            if (!this.directorChoices.some(val => val === data.director))
              this.directorChoices.push(data.director);
            this.films.push(data);
          }
        }
    )
  }

}
</script>

<style scoped>

</style>