<script>
import axios from "axios";
import { v4 as uuid } from "uuid";
export default {
  data() {
    return {
      categoria: {},
      categorias: [
        // {
        //   id: "ac803529-5e2d-45b2-a907-ca993bb301ea",
        //   categoria: "Romance",
        //   editora: "Azul",
        // },
        // {
        //   id: "f0532191-04bb-44fe-be8b-8449ce321169",
        //   categoria: "Romance",
        //   editora: "Rocco",
        // },
        // {
        //   id: "29eaba8d-a376-485e-9509-5dddd6587f0d",
        //   categoria: "Terror",
        //   editora: "Página",
        // },
        // {
        //   id: "d7c068f0-928f-4858-9ae3-eadc32edb230",
        //   categoria: "Suspense",
        //   editora: "Arcoíris",
        // },
      ],
    };
  },
  async created() {
    const categorias = await axios.get("http://localhost:4000/categorias");
    this.categorias = categorias.data;
  },
  methods: {
    async salvar() {
      const categoria_criada = await axios.post(
        "http://localhost:4000/categorias",
        this.categoria
      );
      this.categorias.push(categoria_criada.data);
      this.categoria = {};
    },
    async excluir(categoria) {
      await axios.delete(`http://localhost:4000/categorias/${categoria.id}`);
      const indice = this.categorias.indexOf(categoria);
      this.categorias.splice(indice, 1);
    },
    alerta() {
      alert("ok");
    },
  },
};
</script>

<template>
  <div class="conteudo">
    <div class="titulo">
      <h2>Gerencimento de Categorias</h2>
    </div>
    <div class="input-form">
      <div class="input-group">
        <select
          class="form-select"
          id="inputGroupSelect04"
          aria-label="Example select with button addon"
          v-model="categoria.nome"
          @keyup.enter="salvar"
          placeholder="Categorias"
        >
          <option selected class="disabled" value="">Categorias</option>
          <option value="Romance">Romance</option>
          <option value="Terror">Terror</option>
          <option value="Suspense">Suspense</option>
          <option value="Distopia">Distopia</option>
          <option value="Clássico">Clássico</option>
          <option value="Ação">Ação</option>
          <option value="Ficção-Científica">Ficção-Científica</option>
        </select>

        <select
          class="form-select"
          id="inputGroupSelect04"
          aria-label="Example select with button addon"
          v-model="categoria.editora"
          @keyup.enter="salvar"
        >
          <option selected class="disabled" value="">Editoras</option>
          <option value="Azul">Azul</option>
          <option value="Rocco">Rocco</option>
          <option value="Página">Página</option>
          <option value="Arcoíris">Arcoíris</option>
        </select>
      </div>
      <button @click="salvar">Salvar</button>
    </div>
    <div class="itens-lista">
      <table>
        <thead>
          <tr>
            <td>ID</td>
            <td>Categorias</td>
            <td>Editoras</td>
            <td>Ações</td>
          </tr>
        </thead>
        <tbody>
          <tr v-for="categoria in categorias" :key="categoria.id">
            <td>{{ categoria.id }}</td>
            <td>{{ categoria.nome }}</td>
            <td>{{ categoria.editora }}</td>
            <td>
              <button class="botao" @click="alerta">Editar</button>
              <button class="botao" @click="excluir(categoria)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style></style>
