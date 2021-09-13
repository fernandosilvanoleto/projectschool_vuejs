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
          <td>{{ aluno.id }}</td>
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

      professorid: this.$route.params.prof_id, // DEFINE PRIMEIRO A ROTA NO ARQUIVO router.js > ISSO AQUI É PARA PASSAR NO COMPONENT DE ALUNO O ID DO PROFESSOR E BUSCAR OS SEUS ALUNOS CONFORME O ID PROFESSOR
        // PEGAR O PARÂMETRO DA ROTA
      nome: '',
      alunos: []
    }
  },
  created() {
    // ID DE PROFESSOR ESTÁ PREENCHIDO, PEGAR O ID DELE
    if(this.professorid) {
      this.$http
      .get("http://localhost:3000/alunos?professor.id=" + this.professorid)
      .then(res => res.json()) // passa o retorno para o próximo né
      .then(alunos => this.alunos = alunos);
    } else {
      this.$http
      .get('http://localhost:3000/alunos')
      .then(res => res.json()) // passa o retorno para o próximo né
      .then(alunos => this.alunos = alunos);
    }    
  },
  props: {
  },
  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome,
        sobrenome: ""
      }
      this.$http
      .post('http://localhost:3000/alunos', _aluno)
      .then(res => res.json())
      .then(alunoReturn => {
        this.alunos.push(alunoReturn);
        this.nome = '';   
      })
         
    },
    remover(aluno) {

      this.$http
      .delete(`http://localhost:3000/alunos/${aluno.id}`)
      .then(() => {
        let indice = this.alunos.indexOf(aluno)
        this.alunos.splice(indice, 1);
      })      
    }
  },
}
</script>

<style scoped>
input {
  width: calc(100% - 195px);
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline;
}
.btnInput {
  width: 150px;
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
