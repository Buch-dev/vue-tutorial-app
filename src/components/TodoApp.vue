<template>
  <div class="wrapper flex flex-col gap-10 pt-10">
    <h1 class="text-6xl font-bold">Todo App</h1>
    <div class="flex">
      <input type="text" placeholder="Add a new task" v-model="newTask" />
      <button @click="addTask">Add Task</button>
    </div>
    <ul>
      <li v-for="(task, index) in tasks" :key="index" class="my-2 border-green-950 border-2 pl-3">
        <p>{{ task }}</p>
        <button
          @click="removeTask(index)"
          class="bg-red-700 text-white px-4 py-2"
        >
          Delete
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
import { onMounted, ref } from "vue";

export default {
  setup() {
    const newTask = ref("");
    const tasks = ref([]);
    // Load tasks from local storage on component mount
    onMounted(() => {
      const savedTasks = JSON.parse(localStorage.getItem("tasks")) || [];
      tasks.value = savedTasks;
    });

    const addTask = () => {
      if (newTask.value.trim() !== "") {
        tasks.value.push(newTask.value);
        newTask.value = "";
        // update local storage
        localStorage.setItem("tasks", JSON.stringify(tasks.value));
      }
    };

    const removeTask = index => {
      tasks.value.splice(index, 1);
    };

    return {
      newTask,
      tasks,
      addTask,
      removeTask,
    };
  },
};
</script>

<style scoped>
.wrapper h1 {
  text-align: center;
  color: hsla(160, 100%, 37%, 1);
}

.wrapper .flex {
  display: flex;
  gap: 1rem;
  align-items: center;
  justify-content: center;
  font-size: 16px;
}

.wrapper .flex button {
  height: 35px;
  border-radius: 10px;
  padding-inline: 0.5rem;
  border: none;
  color: #fff;
  background: hsla(160, 100%, 37%, 1);
}

.wrapper .flex input {
  padding-inline-start: 1rem;
  outline: none;
  width: 450px;
  height: 35px;
  border-radius: 10px;
  border: 1px solid green;
}

.wrapper ul {
  padding: 0;
}

.wrapper li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  list-style-type: none;
}
</style>
