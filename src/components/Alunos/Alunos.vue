<template>
  <div>
    <titulo texto="Alunos" />
    <div>
    <input type="text" placeholder="Nome do aluno" v-model="nome"
    v-on:keyup.enter="addAluno()">
    <button class="btn btnInput" @click="addAluno()">Adicionar</button>
    </div>
    <table border="1px">
      <thead>
        <th>Matricula</th>
        <th>Nome</th>
        <th>Opções</th>
      </thead>
      <tbody v-if="alunos.length">
        <tr v-for="(aluno, index) in alunos" :key="index">
          <td>{{ index+1 }}</td>
          <td>{{ aluno.nome }}  {{ aluno.sobrenome }}</td>
          <td>
          <button class="btn btn_Danger" @click="remover(aluno)">Remover</button>
          </td>
        </tr>
      </tbody>
      <tfoot v-if="!alunos.length">
        Nenhum aluno encontrado
      </tfoot>
    </table>
  </div>
</template>

<script>
import Titulo from '../_shared/Titulo';

export default {
  components: {
    Titulo
  },
  data() {
    return {
      titulo: 'Aluno',
      nome: '',
      alunos: []
    }
  },
  created() {
    this.$http
    .get('http://localhost:3000/alunos')
    .then(res => res.json()) // passa o retorno para o próximo né
    .then(alunos => this.alunos = alunos);
  },
  props: {
  },
  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome
      }
      this.alunos.push(_aluno);
      this.nome = '';
      this.alunos.forEach(aluno => {
        console.log(aluno);
      });      
    },
    remover(aluno) {
      let indice = this.alunos.indexOf(aluno)
      this.alunos.splice(indice, 1);
    }
  },
}
</script>

<style scoped>
input {
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline;
}
.btnInput {
  border: 0px;
  padding: 20px;
  display: inline;
  font-size: 1.3em;
  background-color: rgb(116, 115, 115);
}
.btnInput:hover {
  padding: 20x;
  margin: 0px;
  border: 0px;
}
</style>
