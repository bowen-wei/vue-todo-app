<!-- partie html -->
<template>
  <main
    class="container d-flex flex-column align-items-center mt-5 py-3"
    style="max-width: 550px"
  >
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
    <p class="align-self-start fs-5">En Cours</p>
    <VueDraggable ref="el" v-model="todos" class="container-fluid mb-4">
      <li
        v-for="t in todos.filter((t) => !t.fini)"
        :key="t.id"
        :class="{ fini: t.fini }"
        class="d-flex align-item-center justify-content-between p-3"
        style="position: relative"
      >
        <button
          data-bs-toggle="modal"
          :data-bs-target="`#${t.id}modal`"
          style="
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            opacity: 0;
            z-index: 0;
          "
        ></button>
        <div style="z-index: 999" class="d-flex align-items-center gap-2">
          <input
            type="checkbox"
            :id="t.id + 'check'"
            :checked="t.fini"
            @change="toggleFini(t, $event)"
          />
          <span>{{ t.title }}</span>
        </div>
        <span style="color: gray; font-size: 12px" v-if="t.dateLimit"
          ><i class="bi bi-calendar"></i> {{ t.dateLimit }}</span
        >
      </li>
    </VueDraggable>
    <p class="align-self-start fs-5">Finis</p>
    <VueDraggable ref="el" v-model="todos" class="list-group container-fluid">
      <li
        v-for="t in todos.filter((t) => t.fini)"
        :key="t.id"
        :class="{ fini: t.fini }"
        class="list-group-item d-flex align-items-center justify-content-between"
        style="position: relative"
      >
        <button
          data-bs-toggle="modal"
          :data-bs-target="`#${t.id}modal`"
          style="
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            opacity: 0;
            z-index: 0;
          "
        ></button>
        <div style="z-index: 999" class="d-flex align-items-center">
          <input
            type="checkbox"
            :id="t.id + 'check'"
            :checked="t.fini"
            @change="toggleFini(t, $event)"
          />
          <span>{{ t.title }}</span>
        </div>
      </li>
    </VueDraggable>

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

    <!-- Modal -->
    <div
      v-for="t in todos"
      class="modal fade"
      :id="t.id + 'modal'"
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
              Modifier le todo
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="modifierTodo(t, $event)">
              <div class="mb-3">
                <label for="titre" class="form-label">Titre</label>
                <input
                  type="text"
                  name="titre"
                  class="form-control"
                  id="titre"
                  :value="t.title"
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
                  :value="t.description"
                ></textarea>
              </div>
              <div class="mb-3">
                <label for="date" class="form-label">Date Limite</label>
                <input
                  type="date"
                  class="form-control"
                  id="date"
                  :value="t.dateLimit"
                />
              </div>
              <button
                type="submit"
                class="btn btn-primary"
                data-bs-dismiss="modal"
              >
                Modifier
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
import { VueDraggable } from "vue-draggable-plus";

const title = ref("");
const description = ref("");
const dateLimit = ref("");

const todos = ref([]);

function test() {
  console.log("hi");
}

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

function modifierTodo(t, e) {
  console.log(e.target.titre.value);
  console.log(e.target.description.value);
  console.log(e.target.date.value);
  todos.value = todos.value.map((todo) => {
    if (todo.id === t.id) {
      return {
        ...todo,
        title: e.target.titre.value,
        description: e.target.description.value,
        dateLimit: e.target.date.value,
      };
    }
    return todo;
  });
  localStorage.setItem("todos", JSON.stringify(todos.value));
}

function supprimerTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo);
  localStorage.setItem("todos", JSON.stringify(todos.value));
}

function toggleFini(t, e) {
  setTimeout(() => {
    t.fini = e.target.checked;
    localStorage.setItem("todos", JSON.stringify(todos.value));
  }, 500);
}
</script>

<style>
.fini {
  text-decoration: line-through;
  color: gray;
}
input[type="checkbox"] {
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 1px solid lightgray;
  margin: auto !important;
}

input[type="checkbox"]:checked {
  background-color: darkseagreen;
  border-color: darkseagreen;
  position: relative;
}

input[type="checkbox"]:checked::after {
  content: "\2713";
  color: white;
  font-weight: bold;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

li {
  background-color: white;
  margin-bottom: 1rem;
  border-radius: 5px;
  -webkit-box-shadow: 0px 4px 12px -2px rgba(0, 0, 0, 0.38);
  box-shadow: 0px 4px 12px -2px rgba(0, 0, 0, 0.38);
}
</style>
