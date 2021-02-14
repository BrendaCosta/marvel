<template>
  <main>
    <!-- barra inicial -->
    <header>
      <nav>
        <h2>Movies</h2>
      </nav>
    </header>

    <!-- Exibir nos resultados da busca: nome, cartaz, data de lançamento, sinopse, pontuação do filme e categorias -->

    <section class="procurar">
      <input
        type="search"
        id="barraPesquisa"
        v-model="buscarFilme"
        v-on:keyup.enter="find(1)"
        placeholder="Procure pelo nome do filme"
      />
    </section>

    <!-- Exibição dos resultados da busca -->
    <div class="container">
      <div v-for="item in filme" :key="item.id" class="filme-box">
        <div class="imagemFilme">
          <p class="circulo">{{ item.vote_average }}</p>
          <img
            :src="'http://image.tmdb.org/t/p/w185' + item.poster_path"
            alt=""
          />
        </div>
        <div class="infoFilme">
          <h2 class="titulo">{{ item.title }}</h2>

          <!-- <p class="circulo">{{ item.vote_average }}</p> -->
          <p>Lançado em: {{ item.release_date }}</p>
          <p class="sinopse">{{ item.overview }}</p>
          <p>
            <!-- <span>GENERO {{ getGenero(item.genre_ids) }}</span> -->
          </p>
        </div>
      </div>

      <div>
        <!--Paginação -->
        <div
          v-if="paginaAtual > 1 && paginaAtual"
          v-on:click="find(paginaAtual - 1)"
        >
          {{ paginaAtual - 1 }}
        </div>
        <div v-if="paginaAtual">
          {{ paginaAtual }}
        </div>
        <div
          v-if="paginaAtual && paginaAtual < totalDePaginas"
          v-on:click="find(paginaAtual + 1)"
        >
          {{ paginaAtual + 1 }}
        </div>
        <div
          v-if="paginaAtual && paginaAtual < totalDePaginas"
          v-on:click="find(paginaAtual + 2)"
        >
          {{ paginaAtual + 2 }}
        </div>
        ...
        <div
          v-if="paginaAtual && paginaAtual != totalDePaginas"
          v-on:click="find(totalDePaginas)"
        >
          {{ totalDePaginas }}
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import api from "@/services/api.js";

export default {
  name: "Header",
  components: {},

  data() {
    return {
      filme: [],
      buscarFilme: "",
      paginaAtual: false,
      genres: [],
    };
  },
  computed: {
    resultadoBusca: function () {
      if (this.buscarFilme == "" || this.buscarFilme == " ") {
        return this.filme;
      } else {
        return this.filme.filter((filme) => filme.title == this.buscarFilme);
      }
    },
  },
  methods: {
    find: function (page) {
      api
        .get(
          "https://api.themoviedb.org/3/search/movie?api_key=7d79b7b222172651eccb31f77ec49bd3&page=" +
            page +
            "&language=pt-BR&query=" +
            this.buscarFilme
        )
        .then((response) => {
          this.filme = response.data.results;
          this.paginaAtual = response.data.page;
          this.totalDePaginas = response.data.total_pages;
          console.log(response.data);
        });
    },
    getGenero(ids) {
      let retorno = [];
      ids.forEach((out_id) => {
        this.genres.filter((inner_id) => inner_id.id == out_id);
      });
      console.log(retorno);
      return retorno;
    },
  },
  mounted() {
    api
      .get(
        "https://api.themoviedb.org/3/genre/movie/list?api_key=7d79b7b222172651eccb31f77ec49bd3"
      )
      .then((response) => {
        // this.genre = response.data;
        console.log(response.data.genres.name);
      });
  },
};
</script>

<style scoped>
header {
  background-color: var(--color-background-nav);
  height: 60px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-content: center;
}

nav {
  display: flex;
  justify-content: space-between;
  height: 60px;
  align-items: center;
  color: var(--color-text-title);
}
.filme-box {
  /* display: flex; */
  margin: 20px;
  background-color: var(--color-background-home);
}

.procurar {
  margin-top: 20px;
}

#barraPesquisa {
  margin: 0 auto;
  padding: 10px;
  border: none;
  margin-bottom: 10px;
  border-radius: 50px;
  text-align: center;
  outline: none;
  font-size: 16px;
  background-color: var(--color-background-home);
}
.btnBuscar {
  padding: 10px;

  margin: 0 auto;
  margin-left: 5px;
  border-radius: 50px;
  border: none;
  text-transform: uppercase;
  font-weight: 700;
  color: #fff;
  background: linear-gradient(to right, #116193, #00e4e1);
  cursor: pointer;
  outline: none;
}

.infoFilme {
  width: 100%;
}
.infoFilme p {
  font-size: 18px;
  padding: 25px;
  text-align: left;
}
.titulo {
  background-color: var(--color-background-nav);
  color: var(--color-text-light);
}
.circulo {
  background: var(--color-background-nav);
  color: var(--color-background);
  border: 2.5px solid var(--color-text-title);
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  margin: 0 10px;
  position: absolute;
  transform: scale3d(1.5, 1.5, 1.5);
  margin-top: 220px;
  margin-left: 230px;
}

@media (min-width: 700px) {
  #barraPesquisa {
    width: 40%;
  }
  .btnBuscar {
    width: 10%;
  }
  .filme-box {
    display: flex;
  }
  .circulo {
    margin-top: 20px;
    margin-left: 170px;
  }
}
</style>