<script setup>

import { reactive, onMounted } from 'vue';

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

<header>
  <div class = "container p-5 mb-4 mt-4 bg-light rounded-3">
    <h1>Minhas tarefas</h1>
    <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
  </div>
</header>
<main>
  <div class = "container">
    <form @submit.prevent = "cadastraTarefa">
      <div class = "row">
        <div class = "col">
          <input v-bind:value = "estado.tarefaTemp" @change = "evento => estado.tarefaTemp = evento.target.value"  required type = "text" class = "form-control" placeholder = "Digite aqui a descrição da tarefa">
        </div>
        <div class = "col-md-2">
          <button class = "btn btn-primary" type = "submit">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change = "evento => estado.filtro = evento.target.value" class = "form-control">
            <option value = "todas">Todas tarefas</option>
            <option value = "pendentes">Pendentes</option>
            <option value = "finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class = "list-group mt-4">
      <li v-bind:key = "index" v-for = "(tarefa, index) in getTarefasFiltradas()" class = "list-group-item d-flex justify-content-between align-items-center">
        <div>
          <input @change = "evento => tarefa.finalizada = evento.target.checked" v-bind:checked = "tarefa.finalizada" v-bind:id = "tarefa.titulo" type="checkbox">
          <label v-bind:class = "{ done: tarefa.finalizada }" class = "ms-3" v-bind:for = "tarefa.titulo">{{ tarefa.titulo }}</label>
        </div>
        <button @click = "() => deletaTarefa(index)" class = "btn btn-danger btn-sm">Deletar</button>
      </li>
    </ul>
  </div>
</main>

</template>

<style scoped>

.done {
  text-decoration: line-through;
}

</style>
