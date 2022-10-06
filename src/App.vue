<template>
  <main class="app">
    <section class="greeting">
      <h3 class="title">✍️ToDo Application</h3>
    </section>

    <div class="input-section">
      <section class="create-todo">
        <form @submit.prevent="addTodo">
          <h3>What's do you plan on doing?</h3>
          <input
            type="text"
            placeholder="e.g. email your boss"
            v-model="text"
          />

          <input type="submit" value="Add todo" />
        </form>
      </section>
    </div>
    <div class="todo-section">
      <section class="todo-list">
        <div class="list">
          <div
            v-for="todo in todos"
            :class="`todo-item ${todo.done && 'done'}`"
          >
            <label>
              <input type="checkbox" v-model="todo.done" />
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.todo" />
            </div>

            <div class="actions">
              <button class="delete" @click="deleteTodo(todo)">Delete</button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
// ref --> for state management
// onMounted --> to execute a command once the page starts
// computed --> for mathematical computing
// watch --> an observable which watches for page changes

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
  (newValue) => {
    localStorage.setItem("todos", JSON.stringify(newValue));
  },
  { deep: true }
);

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>
