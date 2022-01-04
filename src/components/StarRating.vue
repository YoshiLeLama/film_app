<template>
  <span :title="rating.toString().concat('/100')">
    <i class="bi bi-star-fill" v-for="n in Math.floor(rating / (100 / max_stars))" :key="n"></i>
    <i class="bi bi-star-half"
       v-if="rating / max_stars - Math.floor(rating / max_stars) < 50 / max_stars && rating % 100"></i>
    <i class="bi bi-star" v-for="n in Math.floor((100 - rating) / (100 / max_stars))" :key="n + 5"></i>
  </span>
</template>

<script>
export default {
  name: "StarRating",
  props: {
    // Rating from 0 to 100
    rating: Number,
    max_stars: Number
  }
}
</script>

<style scoped>

</style>