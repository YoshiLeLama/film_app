<template>
  <form class="d-flex flex-row-reverse p-3" action="#">
    <div class="col-auto ms-3">
      <input type="text" class="form-control position-relative end-0" id="inputTitle" placeholder="Filter titles"
             v-model="titleFilter"
             @input="$emit('titleFilterChange', titleFilter)">
    </div>
    <div class="col-auto ms-3">
      <select class="form-select" v-model="directorChoice" @change="$emit('directorFilterChange', directorChoice)">
        <option value="">Filter by director...</option>
        <option v-for="value in director_choices" :value="value" :key="value">{{ value }}</option>
      </select>
    </div>
    <div class="col-auto ms-3">
      <select class="form-control" v-model="sortingChoice" @change="$emit('sortingChange', sortingChoice)">
        <option value="">Sort by...</option>
        <option v-for="value in sorting_choices" :value="value" :key="value">{{ value }}</option>
      </select>
    </div>
  </form>
</template>

<script>
export default {
  name: "ListFilter",
  data() {
    return {
      titleFilter: "",
      directorChoice: "",
      sortingChoice: ""
    }
  },
  props: {
    director_choices: Array,
    sorting_choices: Array
  },
  emits: [
    "titleFilterChange",
    "directorFilterChange",
    "sortingChange"
  ]
}
</script>

<style scoped>

</style>