<script>
import axios from "axios";
export default {
  data() {
    return {
      novo_autor: "",
      novo_idade: "",
      novo_editora: "",
      novo_categoria: "",
      autores: [],
    };
  },
  async created() {
    const autores = await axios.get("http://localhost:4000/autores");
    this.autores = autores.data;
  },
  methods: {
    async salvar() {
      if (this.novo_autor !== "") {
        const autor = {
          name: this.novo_autor,
          idadeid: this.novo_idade,
          editoracod: this.novo_editora,
          categoria: this.novo_categoria,
        };
        const autor_criado = await axios.post(
          "http://localhost:4000/autores",
          autor
        );
        this.autores.push(autor_criado.data);
        this.novo_autor = "";
        this.novo_idade = "";
        this.novo_editora = "";
        this.novo_categoria = "";
      }
    },
    async excluir(autor) {
      await axios.delete(`http://localhost:4000/autores/${autor.id}`);
      const indice = this.autores.indexOf(autor);
      this.autores.splice(indice, 1);
    },
  },
};
</script>
<template>
  <div class="container">
    <div class="title">
      <h2>Gerenciamento de Autores</h2>
    </div>
    <div class="form-input">
      <input type="text" placeholder="Nome" v-model="novo_autor" @keypress.enter="salvar" />
      <input type="text" placeholder="Idade" v-model="novo_idade" @keypress.enter="salvar" />
      <input type="text" placeholder="Código Editora" v-model="novo_editora" @keypress.enter="salvar" />
      <input type="text" placeholder="Gênero" v-model="novo_categoria" @keypress.enter="salvar" />
      <button @click="salvar">Salvar</button>
    </div>
    <div class="list-items">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Idade</th>
            <th>Código Editora</th>
            <th>Categoria do Livro</th>
            <th>Ação</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="autor in autores" :key="autor.id">
            <td>{{ autor.id }}</td>
            <td>{{ autor.name }}</td>
            <td>{{ autor.idadeid }}</td>
            <td>{{ autor.editoracod }}</td>
            <td>{{ autor.categoria }}</td>
            <td>
              <button @click="excluir(autor)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<style>
</style>