<template>
  <div id="app">
    <p class="fs-2">Filmflix</p>
    <FilmList></FilmList>
  </div>
</template>

<script>


import FilmList from "@/components/FilmList";

export default {
  name: 'App',
  components: {FilmList}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.no-select {
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}
</style>
