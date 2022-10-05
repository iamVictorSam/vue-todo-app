<template>
  <main class="app">
    <section class="greeting">
      <h3 class="title">😊ToDo Application</h3>
    </section>

    <div class="input-section">
      <section class="create-task">
        <form @submit.prevent="addTask">
          <h3>What's do you plan on doing?</h3>
          <input
            type="text"
            placeholder="e.g. email your boss"
            v-model="input_text"
          />

          <input type="submit" value="Add Task" />
        </form>
      </section>
    </div>
    <div class="todo-section">
      <section class="task-list">
        <h3>Todos</h3>
        <div class="list">
          <div
            v-for="task in tasks"
            :class="`task-item ${task.completed && 'done'}`"
          >
            <label>
              <input type="checkbox" v-model="task.completed" />
            </label>

            <div class="task-content">
              <input type="text" v-model="task.task" />
            </div>

            <div class="actions">
              <button class="delete" @click="removeTask(task)">Delete</button>
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

const tasks = ref([]);
const name = ref("");
const input_text = ref("");
const input_category = ref(null);

// const tasks_asc = computed(() => tasks.value.sort((a, b) => {
//   return b.createdAt - a.createdAt;
// }))

const addTask = () => {
  if (input_text.value.trim() === "" || input_category === null) {
    return;
  }

  tasks.value.unshift({
    task: input_text.value,
    category: input_category.value,
    completed: false,
    // createdAt: new Date().getTime(),
  });

  input_text.value = "";
  input_category.value = null;
};

watch(
  tasks,
  (newValue) => {
    localStorage.setItem("tasks", JSON.stringify(newValue));
  },
  { deep: true }
);

const removeTask = (task) => {
  tasks.value = tasks.value.filter((x) => x !== task);
};

watch(name, (newValue) => {
  localStorage.setItem("name", newValue);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  tasks.value = JSON.parse(localStorage.getItem("tasks")) || [];
});
</script>
