<script>
import axios from "axios";
export default {
  data() {
    return {
      novo_editora: "",
      novo_codigo: "",
      editoras: [],
    };
  },
  async created() {
    const editoras = await axios.get("http://localhost:4000/editoras");
    this.editoras = editoras.data;
  },
  methods: {
    async salvar() {
      if (this.novo_editora !== "") {
        const editora = {
          name: this.novo_editora,
          codigo: this.novo_codigo,
        }
        const editora_criado = await axios.post(
          "http://localhost:4000/editoras",
          editora
        );
        this.editoras.push(editora_criado.data);
        this.novo_editora = "";
        this.novo_codigo = "";
      }
    },
    async excluir(editora) {
      await axios.delete(`http://localhost:4000/editoras/${editora.id}`);
      const indice = this.editoras.indexOf(editora);
      this.editoras.splice(indice, 1);
    },
  },
};
</script>
<template>
  <div class="container">
    <div class="title">
      <h2>Gerenciamento de Editoras</h2>
    </div>
    <div class="form-input">
      <input type="text" placeholder="Nome" v-model="novo_editora" @keypress.enter="salvar" />
      <input type="text" placeholder="Código" v-model="novo_codigo" @keypress.enter="salvar" />
      <button @click="salvar">Salvar</button>
    </div>
    <div class="list-items">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Código</th>
            <th>Ação</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="editora in editoras" :key="editora.id">
            <td>{{ editora.id }}</td>
            <td>{{ editora.name }}</td>
            <td>{{ editora.codigo }}</td>
            <td>
              <button @click="excluir(editora)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<style>
</style>