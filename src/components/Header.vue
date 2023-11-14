<script setup>

const props = defineProps(["tarefasPendentes"]);

</script>

<template>

<header>
    <div class = "container p-5 mb-4 mt-4 bg-light rounded-3">
        <h1>Minhas tarefas</h1>
        <p>Você possui {{ props.tarefasPendentes }} tarefas pendentes</p>
    </div>
</header>

</template>