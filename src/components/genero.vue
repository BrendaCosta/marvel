<template>
  <div></div>
</template>
<script>
import api from "@/services/api.js";
export default {
  name: "Genero",
  data() {
    return {
      listagemVideos: [],
    };
  },
  mounted() {
    api
      .get(
        "https://api.themoviedb.org/3/genre/movie/list?api_key=7d79b7b222172651eccb31f77ec49bd3&language=pt-BR"
      )
      .then((response) => {
        console.log(response.data);
      });
  },
};
</script>
<style scoped>
</style>