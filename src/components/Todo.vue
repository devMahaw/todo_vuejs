<script setup>

const props = defineProps(["tarefas", "deletaTarefa"]);

</script>

<template>

<ul class="list-group mt-4">
    <li v-bind:key="index" v-for="(tarefa, index) in props.tarefas"
        class="list-group-item d-flex justify-content-between align-items-center">
        <div>
            <input @change="evento => tarefa.finalizada = evento.target.checked" v-bind:checked="tarefa.finalizada"
                v-bind:id="tarefa.titulo" type="checkbox">
            <label v-bind:class="{ done: tarefa.finalizada }" class="ms-3" v-bind:for="tarefa.titulo">{{ tarefa.titulo
            }}</label>
        </div>
        <button @click="props.deletaTarefa" class="btn btn-danger btn-sm">Deletar</button>
    </li>
</ul>

</template>

<style scoped>

.done {
    text-decoration: line-through;
}

</style>