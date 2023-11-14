<script setup>

import { reactive, onMounted } from 'vue';
import Cabecalho from './components/Header.vue';
import Formulario from './components/Form.vue';
import Lista from './components/Todo.vue';

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
    <Cabecalho tarefas-pendentes = "10"/>
    <Formulario />
    <Lista />
  </div>
</main>

</template>

<style scoped>

.done {
  text-decoration: line-through;
}

</style>
