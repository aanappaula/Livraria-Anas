<script>
import axios from "axios";
import { v4 as uuid } from "uuid";
export default {
  data() {
    return {
      livro: {},
      livros: [
        // {
        //   id: "ac803529-5e2d-45b2-a907-ca993bb301ea",
        //   nome_livro: "Harry Potter e a Pedra  Filosofal",
        //   autor: "J.K Rowling",
        //   categoria: "Romance",
        //   editora: "Azul",
        // },
        // {
        //   id: "f0532191-04bb-44fe-be8b-8449ce321169",
        //   nome_livro: "A sombra do vento",
        //   autor: "Carlos Ruíz Zafón",
        //   categoria: "Terror",
        //   editora: "Rocco",
        // },
        // {
        //   id: "29eaba8d-a376-485e-9509-5dddd6587f0d",
        //   nome_livro: "O caçador de pipas",
        //   autor: "Khaled Hosseini",
        //   categoria: "Mistério",
        //   editora: "Página",
        // },
        // {
        //   id: "d7c068f0-928f-4858-9ae3-eadc32edb230",
        //   nome_livro: "Anjo da escuridão",
        //   autor: "Sidney Sheldon",
        //   categoria: "Suspense",
        //   editora: "Arcoíris",
        // },
      ],
    };
  },
  async created() {
    const livros = await axios.get("http://localhost:4000/livros");
    this.livros = livros.data;
  },
  methods: {
    async salvar() {
      const livro_criado = await axios.post(
        "http://localhost:4000/livros",
        this.livro
      );
      this.livros.push(livro_criado.data);
      this.livro = {};
    },
    async excluir(livro) {
      await axios.delete(`http://localhost:4000/livros/${livro.id}`);
      const indice = this.livros.indexOf(livro);
      this.livros.splice(indice, 1);
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
      <h2>Gerencimento de Livros</h2>
    </div>
    <div class="input-form">
      <input
        type="text"
        v-model="livro.nome"
        @keyup.enter="salvar"
        placeholder="Livros"
      />
      <div class="input-group">
        <select
          class="form-select"
          id="inputGroupSelect04"
          aria-label="Example select with button addon"
          v-model="livro.autor"
          @keyup.enter="salvar"
          placeholder="Autores"
        >
          <option selected class="disabled" value="">Autores</option>
          <option value="J.K Rowling">J.K Rowling</option>
          <option value="Carlos Ruíz Zafón">Carlos Ruíz Zafón</option>
          <option value="Khaled Hosseini">Khaled Hosseini</option>
          <option value="Sidney Sheldon">Sidney Sheldon</option>
        </select>
        <select
          class="form-select"
          id="inputGroupSelect04"
          aria-label="Example select with button addon"
          v-model="livro.categoria"
          @keyup.enter="salvar"
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
          v-model="livro.editora"
          @keyup.enter="salvar"
        >
          <option selected class="disabled" value="">Editoras</option>
          <option value="Azul">Azul</option>
          <option value="Rocco">Rocco</option>
          <option value="Página">Página</option>
          <option value="Arcoíris">Arcoíris</option>
        </select>
        <button @click="salvar">Salvar</button>
      </div>
    </div>

    <div class="itens-lista">
      <table>
        <thead>
          <tr>
            <td>ID</td>
            <td>Livros</td>
            <td>Autores</td>
            <td>Categorias</td>
            <td>Editoras</td>
            <td>Ações</td>
          </tr>
        </thead>
        <tbody>
          <tr v-for="livro in livros" :key="livro.id">
            <td>{{ livro.id }}</td>
            <td>{{ livro.nome }}</td>
            <td>{{ livro.autor }}</td>
            <td>{{ livro.categoria }}</td>
            <td>{{ livro.editora }}</td>

            <td>
              <button class="botao" @click="alerta">Editar</button>
              <button class="botao" @click="excluir(livro)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style>
.disabled {
  color: gray;
}
</style>
