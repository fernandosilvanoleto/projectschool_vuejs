<template>
<div>
    <titulo texto="Professor" />
    <div>
    <input type="text" placeholder="Nome do aluno" v-model="nome"
    v-on:keyup.enter="addAluno()">
    <button class="btn btnInput" @click="addAluno()">Adicionar</button>
    </div>
    <table border="1px">
      <thead>
        <th>Código</th>
        <th>Nome</th>
        <th>Alunos</th>
      </thead>
      <tbody v-if="Professores.length">
        <tr v-for="(professor, index) in Professores" :key="index">
          <td>{{ professor.id }}</td>
          
          <router-link 
          v-bind:to="`/alunos/${professor.id}`" 
          tag="td" 
          style="cursor: pointer;">
              {{ professor.nome }}     
          </router-link>

          <td>
            {{ professor.qtdAlunos }}
          </td>
        </tr>
      </tbody>
      <tfoot v-if="!Professores.length">
        Nenhum professor encontrado
      </tfoot>
    </table>
  </div>
</template>

<script>
import Titulo from '../_shared/Titulo'

export default {
    components: {
        Titulo
    },
    data() {
        return {
            Alunos: [],
            Professores: []
        }
    },
     created() {
    this.$http
    .get('http://localhost:3000/alunos')
    .then(res => res.json()) // passa o retorno para o próximo né
    .then(alunos => {
      this.Alunos = alunos,
      this.carregarProfessores();
      });
    },
    props: {
    },
    methods: {
      pegarQtdAlunosPorProfessor() {
        this.Professores.forEach((professor, index) => {
           professor = {
             id: professor.id,
             nome: professor.nome,
             qtdAlunos: this.Alunos.filter(aluno => aluno.professor.id == professor.id).length
           }
           this.Professores[index] = professor;
        });
      },
      carregarProfessores() {
        this.$http
          .get('http://localhost:3000/professores')
          .then(res => res.json()) // passa o retorno para o próximo né
          .then(professor => {
              this.Professores = professor;
              this.pegarQtdAlunosPorProfessor();
            });
      }
    }
}
</script>

<style>

</style>