<template>
  <main>
    <!-- heading -->
    <header>
      <img src="https://pinia.vuejs.org/logo.svg" alt="pinia logo" />
      <h1>Pinia Tasks</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'">All tasks</button>
      <button @click="filter = 'favs'">Fav tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="loading">Loading tasks...</div>

    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ totalCount }} tasks left to do.</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>You have {{ favCount }} tasks in your favs list.</p>
      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <button @click="taskStore.$reset">reset the state</button>
  </main>
</template>

<script setup>
import { storeToRefs } from "pinia";
import TaskDetails from "./components/TaskDetails.vue";
import TaskForm from "./components/TaskForm.vue";
import { useTaskStore } from "./stores/TaskStore";
import { ref } from "vue";

const taskStore = useTaskStore();

//convertimos los elementos del store (state & getters) en refs [ACTIONS NOT PERMITTED THIS WAY].
//Y asi podemos usarlos directamente en el template
//en vez de {{taskStore.totalCount}}, usaremos {{totalCount}} directamente
const { tasks, isLoading, favs, total, totalCount, favCount } =
  storeToRefs(taskStore);

//fetch tasks
taskStore.getTasks();

const filter = ref("all");
</script>
