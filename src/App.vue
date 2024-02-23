<!-- partie html -->
<template>
  <main
    class="container border d-flex flex-column align-items-center mt-5 py-3"
    style="max-width: 550px"
  >
    <!-- <form @submit.prevent="ajouterTodo()">
      <input v-model="newTodo" type="text" placeholder="nouveau todo..." />
      <button class="btn btn-primary">Ajouter</button>
    </form> -->
    <div
      class="container-fluid d-flex align-items-center justify-content-between mb-3"
    >
      <span class="fs-3 fw-bold">TODO</span>
      <button
        class="btn btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#staticBackdrop"
      >
        Ajouter
      </button>
    </div>
    <ul class="list-group container-fluid">
      <li
        v-for="t in todos"
        :key="t.id"
        :class="{ fini: t.fini }"
        class="list-group-item d-flex align-item-center justify-content-between"
      >
        <div>
          <input
            type="checkbox"
            :id="t.id + 'check'"
            :checked="t.fini"
            @change="toggleFini(t, $event)"
          />
          <span>{{ t.title }}</span>
        </div>

        <button @click="supprimerTodo(t)" class="btn btn-danger">
          <i class="bi bi-x-lg"></i>
        </button>
      </li>
    </ul>
    <p v-if="todos.length === 0">Pas de Todo !</p>

    <!-- Modal -->
    <div
      class="modal fade"
      id="staticBackdrop"
      data-bs-backdrop="static"
      data-bs-keyboard="false"
      tabindex="-1"
      aria-labelledby="staticBackdropLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="staticBackdropLabel">
              Ajouter un todo
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="ajouterTodo()">
              <div class="mb-3">
                <label for="titre" class="form-label">Titre</label>
                <input
                  type="text"
                  name="titre"
                  class="form-control"
                  id="titre"
                  v-model="title"
                />
              </div>
              <div class="mb-3">
                <label for="description" class="form-label">
                  Déscription
                </label>
                <textarea
                  class="form-control"
                  name="description"
                  id="description"
                  v-model="description"
                ></textarea>
              </div>
              <div class="mb-3">
                <label for="date-limite" class="form-label">Date Limite</label>
                <input
                  type="date"
                  class="form-control"
                  id="date-limite"
                  v-model="dateLimit"
                />
              </div>
              <button
                class="btn btn-primary"
                data-bs-dismiss="modal"
                :disabled="!title"
              >
                Enregistrer
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<!-- partie js -->
<script setup>
import { ref, onMounted } from "vue";

const title = ref("");
const description = ref("");
const dateLimit = ref("");

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
  if (title.value === "") return;
  console.log("titre : " + title.value);
  console.log("description: " + description.value);
  console.log("date limite: " + dateLimit.value);
  todos.value.push({
    id: new Date().getTime(),
    title: title.value,
    description: description.value,
    dateLimit: dateLimit.value,
    fini: false,
  });
  title.value = "";
  description.value = "";
  dateLimit.value = "";
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
