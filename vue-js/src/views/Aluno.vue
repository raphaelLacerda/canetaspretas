<template>
  <div class="container">
    <form method="post">
      <div class="mb-3 row">
        <label class="col-sm-2 col-form-label">Nome:</label>
        <div class="col-sm-10">
          <input
            v-model="aluno.nome"
            name="nome"
            type="text"
            class="form-control"
          />
        </div>
      </div>
      <div class="mb-3 row">
        <label class="col-sm-2 col-form-label">Email:</label>
        <div class="col-sm-10">
          <input
            v-model="aluno.email"
            name="email"
            type="email"
            class="form-control"
          />
        </div>
      </div>
      <div class="col-auto">
        <button
          @click.prevent="save"
          type="submit"
          class="btn btn-lg btn-outline-primary mb-3"
        >
          Salvar
        </button>
      </div>
    </form>
    {{ data.value }}
    <div id="listaAlunos" v-if="alunos.length > 0" class="row mt-2">
      <h3>Listagem de Alunos - total de alunos {{ alunos.length }}</h3>
      <ul>
        <li class="mt-3" v-for="(aluno, index) in alunos" v-bind:key="index">
          Nome: {{ aluno.nome }} - Email: {{ aluno.email }}
          <button @click="edit(aluno)" class="btn btn-outline-warning">
            editar
          </button>
          <button @click="remove(aluno)" class="btn btn-outline-danger ms-3">
            remover
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { defineComponent } from '@vue/composition-api';
import axios from 'axios';
import { ref } from 'vue';
const API_URL = 'http://localhost:8080/api/v1/alunos';
export default defineComponent({
  props: {},
  data() {
    return {
      alunos: [],
      aluno: {},
    };
  },
  mounted() {
    axios.get(API_URL).then((response) => {
      this.alunos = response.data;
      console.log(this.alunos);
    });
  },
  setup() {
    const data = ref([]);
    return {
      data,
    };
  },
  methods: {
    save() {
      console.log(`Salvando o aluno ${JSON.stringify(this.aluno)}`);
      if (this.aluno.id) {
        axios.put(API_URL + '/' + this.aluno.id, this.aluno);
      } else {
        axios.post(API_URL, this.aluno).then((response) => {
          this.alunos.push(response.data);
        });
      }
      this.aluno = {};
    },
    edit(aluno) {
      this.aluno = aluno;
    },
    remove(aluno) {
      axios.delete(API_URL + '/' + aluno.id).then(() => {
        this.alunos = this.alunos.filter((a) => a.id !== aluno.id);
      });
    },
  },
});
</script>
