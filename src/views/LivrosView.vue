<script>
import axios from "axios";
export default {
  data() {
    return {
      novo_autor: "",
      novo_editora: "",
      novo_categoria: "",
      novo_livro: "",
      livros: [],
    };
  },
  async created() {
    const livros = await axios.get("http://localhost:4000/livros");
    this.livros = livros.data;
  },
  methods: {
    async salvar() {
      if (this.novo_livro !== "") {
        const livro = {
          name: this.novo_livro,
          autor: this.novo_autor,
          editoraid: this.novo_editora,
          categoria: this.novo_categoria,
        };
        const livro_criado = await axios.post(
          "http://localhost:4000/livros",
          livro
        );
        this.livros.push(livro_criado.data);
        this.novo_livro = "";
        this.novo_autor = "";
        this.novo_editora = "";
        this.novo_categoria = "";
      }
    },
    async excluir(livro) {
      await axios.delete(`http://localhost:4000/livros/${livro.id}`);
      const indice = this.livros.indexOf(livro);
      this.livros.splice(indice, 1);
    },
  },
};
</script>
<template>
  <div class="title">
    <h2>Gerenciamento de livros</h2>
  </div>
  <div class="form-input">
    <div class="container">
      <div class="title">
      </div>
      <div class="form-input">
        <input type="text" placeholder="Nome" v-model="novo_livro" @keypress.enter="salvar" />
        <input type="text" placeholder="Autor" v-model="novo_autor" @keypress.enter="salvar" />
        <input type="text" placeholder="Editora" v-model="novo_editora" @keypress.enter="salvar" />
        <input type="text" placeholder="Categoria" v-model="novo_categoria" @keypress.enter="salvar" />
        <button @click="salvar">Salvar</button>
      </div>
    </div>
    </div>
    <div class="list-items">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Autor</th>
            <th>Editora</th>
            <th>Categoria</th>
            <th>Nome</th>
            <th>Ação</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="livro in livros" :key="livro.id">
            <td>{{ livro.id }}</td>
            <td>{{ livro.name }}</td>
            <td>{{ livro.editoraid }}</td>
            <td>{{ livro.categoria }}</td>
            <td>{{ livro.autor }}</td>
            <td>
              <button @click="excluir(livro)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
</template>
<style>
</style>
