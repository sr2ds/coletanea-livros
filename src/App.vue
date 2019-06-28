<template>
  <div id="app">

   <a target="_blank" href="https://github.com/sr2ds/coletanea-livros"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_darkblue_121621.png" alt="Fork me on GitHub"></a>

    <div class="container">
      <h1>Coletanea de Livros</h1>
      <p>Está é uma lista de livros que você pode baixar gratuitamente no formato .mobi (kindle)</p>
      <p>Atenção: Não sou responsável pelos livros, estes arquivos estão divulgados na internet e meu trabalho foi apenas listar. Ou seja, eles podem ficar indisponíveis a qualquer momento.</p>
      <p>O objetivo dessa lista é testar a tecnologia vuejs.</p>

      <input class="form-control" type="text" placeholder="Que livro você procura?" v-model="busca">
      <hr>
      <table
        id="dtBasicExample"
        class="table table-striped table-bordered table-sm"
        cellspacing="0"
        width="70%"
      >
        <thead>
          <tr>
            <th class="th-sm">#</th>
            <th class="th-sm">Nome</th>
            <th class="th-sm">Download .mobi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(livro, index) in livrosFiltrados" :key="livro.gdriveId">
            <td>{{ index+1 }}</td>
            <td>{{ livro.nome }}</td>
            <td>
              <a
                :href="`https://drive.google.com/uc?export=download&id=${livro.gdriveId}`"
                target="_blank"
              >Baixar</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
require("bootstrap/dist/css/bootstrap.min.css");

export default {
  name: "app",
  async mounted() {
    this.montaLista();
  },
  methods: {
    montaLista() {
      const url_livros = "https://gist.githubusercontent.com/sr2ds/4feaee6e81a37e2d5bebf64b7ca41b70/raw/be8947885509ef1fc75b68f408f393db060ac3cd/livros.json";
      axios.get(url_livros).then(({ data }) => {
        this.livros = data;
      });
    },
    geraLink(rowObj) {
      return `<a href="https://drive.google.com/uc?export=download&id=${ rowObj.gdriveId }" target="_blank"> Baixar </a>`;
    }
  },
  computed: {
    livrosFiltrados() {
      if (!this.busca) {
        return this.livros;
      }
      return this.livros
        .filter(livro => {
          return livro.nome;
        })
        .filter(livro => {
          return (
            livro.nome.toUpperCase().indexOf(this.busca.toUpperCase()) > -1
          );
        });
    }
  },
  data() {
    return {
      busca: "",
      livros: []
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
