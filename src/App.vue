<!-- partie html -->
<template>
  <form @submit.prevent="ajouterTodo()">
    <input v-model="newTodo" type="text" placeholder="nouveau todo..." />
    <button>Ajouter</button>
  </form>
  <ol>
    <li v-for="t in todos" :key="t.id" :class="{ fini: t.fini }">
      <input
        type="checkbox"
        :checked="t.fini"
        @change="toggleFini(t, $event)"
      />
      {{ t.description }}
      <button @click="supprimerTodo(t)">Supprimer</button>
    </li>
  </ol>
  <p v-if="todos.length === 0">Pas de Todo !</p>
</template>

<!-- partie js -->
<script setup>
import { ref, onMounted } from "vue";

const newTodo = ref("");

const todos = ref([]);

onMounted(() => {
  const todosStocke = localStorage.getItem("todos");
  if (todosStocke) {
    todos.value = JSON.parse(todosStocke);
  } else {
    todos.value = [];
  }
});

function ajouterTodo() {
  if (newTodo.value === "") return;
  todos.value.push({
    id: new Date().getTime(),
    description: newTodo.value,
    fini: false,
  });
  newTodo.value = "";
  localStorage.setItem("todos", JSON.stringify(todos.value));
}

function supprimerTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo);
  localStorage.setItem("todos", JSON.stringify(todos.value));
}

function toggleFini(t, e) {
  t.fini = e.target.checked;
  localStorage.setItem("todos", JSON.stringify(todos.value));
}
</script>

<style>
.fini {
  text-decoration: line-through;
  color: gray;
}
</style>
