<script>
import axios from "axios";
export default {
  data() {
    return {
      novo_categoria: "",
      novo_editora: "",
      categorias: [],
    };
  },
  async created() {
    const categorias = await axios.get("http://localhost:4000/categorias");
    this.categorias = categorias.data;
  },
  methods: {
    async salvar() {
      if (this.novo_categoria !== "") {
        const categoria = {
          name: this.novo_categoria,
          editoracod: this.novo_editora,
        };
        const categoria_criado = await axios.post(
          "http://localhost:4000/categorias",
          categoria
        );
        this.categorias.push(categoria_criado.data);
        this.novo_categoria = "";
        this.novo_editora = "";
      }
    },
    async excluir(categoria) {
      await axios.delete(`http://localhost:4000/categorias/${categoria.id}`);
      const indice = this.categorias.indexOf(categoria);
      this.categorias.splice(indice, 1);
    },
  },
};
</script>
<template>
  <div class="container">
    <div class="title">
      <h2>Gerenciamento de Categorias</h2>
    </div>
    <div class="form-input">
      <input
        type="text"
        placeholder="Gênero"
        v-model="novo_categoria"
        @keypress.enter="salvar"
      />
      <input
        type="text"
        placeholder="Código Editora"
        v-model="novo_editora"
        @keypress.enter="salvar"
      />
      <button @click="salvar">Salvar</button>
    </div>
    <div class="list-items">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Código Editora</th>
            <th>Ação</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="categoria in categorias" :key="categoria.id">
            <td>{{ categoria.id }}</td>
            <td>{{ categoria.name }}</td>
            <td>{{ categoria.editoracod }}</td>
            <td>
              <button @click="excluir(categoria)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<style></style>
