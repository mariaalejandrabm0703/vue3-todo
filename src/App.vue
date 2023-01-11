<script setup>
import { ref, onMounted, watch } from "vue";
// ref --> para la gestión del estado
// onMounted --> para ejecutar un comando una vez iniciada la página
// computed --> para cálculo matemático
// watch --> un observable que vigila los cambios de página
const todos = ref([]);
const text = ref("");

function addTodo() {
  if (text.value.trim() === "") {
    return;
  }

  todos.value.unshift({
    todo: text.value,
    done: false,
  });

  text.value = "";
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((x) => x !== todo);
}

watch(
  todos,
  (newTodoValue) => {
    localStorage.setItem("todos", JSON.stringify(newTodoValue));
  },
  { deep: true }
);

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h3 class="title">✍️Aplicación todos</h3>
    </section>
    <div class="input-section">
      <section class="create-todo">
        <form @submit.prevent="addTodo">
          <h3>¿Qué piensa hacer🙂?</h3>
          <input type="text" placeholder="ej. lavar ropa" v-model="text" />
          <input type="submit" value="Agregar todo" />
        </form>
      </section>
      <div class="todo-section">
        <section class="todo-list">
          <h2 v-show="todos?.length === 0">No todos aquí😞</h2>
          <div class="list">
            <div
              v-for="(todo, index) in todos"
              :key="index"
              :class="`todo-item ${todo.done && 'done'}`"
            >
              <label>
                <input type="checkbox" v-model="todo.done" />
              </label>
              <div class="todo-content">
                <input type="text" v-model="todo.todo" />
              </div>
              <div class="actions">
                <button class="delete" @click="deleteTodo(todo)">
                  Eliminar
                </button>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div>
  </main>
</template>
