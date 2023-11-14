<script setup>

import { reactive, onMounted } from 'vue';
import Header from './components/Header.vue';
import Form from './components/Form.vue';
import Todo from './components/Todo.vue';

const estado = reactive({
  filtro: "",
  tarefaTemp: "",
  tarefas: []
});

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada);
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada);
}

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case "pendentes":
      return getTarefasPendentes();
    case "finalizadas":
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false
  }

  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = "";
  atualizaLocalStorage();
}

const deletaTarefa = (index) => {
  estado.tarefas.splice(index, 1);
  atualizaLocalStorage();
}

const atualizaLocalStorage = () => {
  localStorage.setItem("tarefas", JSON.stringify(estado.tarefas));
}

onMounted(() => {
  const tarefasSalvas = localStorage.getItem("tarefas");
  if (tarefasSalvas) {
    estado.tarefas = JSON.parse(tarefasSalvas);
  }
});

</script>

<template>

<main>
  <div class = "container">
    <Header v-bind:tarefas-pendentes = "getTarefasPendentes().length" />
    <Form v-bind:trocar-filtro = "evento => estado.filtro = evento.target.value" v-bind:tarefa-temp = "estado.tarefaTemp" v-bind:edita-tarefa-temp = "evento => estado.tarefaTemp = evento.target.value" v-bind:cadastra-tarefa = "cadastraTarefa"/>
    <Todo v-bind:tarefas = "getTarefasFiltradas()" v-bind:deletaTarefa = "() => deletaTarefa(index)"/>
  </div>
</main>

</template>
