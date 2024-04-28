<template>
  <div class="container">
    <div class="wrapped">
      <h1>To Do List</h1>

      <input
        type="text"
        v-model="newTask"
        placeholder="Adding your new todo"
        class="input-task"
      />
      <button @click="addTask" class="btn add">Add Task</button>

      <hr />

      <span v-if="array.length === 0" class="empty">No tasks</span>

      <div class="task-list">
        <div v-for="(item, index) in array" :key="index" class="task-item">
          <div class="task-info">
            <ToDoItem
              :task="item.nome"
              :done="item.done"
              @toggle="toggleTask(index)"
            />
          </div>
          <button @click="deleteTask(index)" class="btn delete">
          <img src="/src/assets/img/trash.svg" alt="Trash" class="trash">
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect, onMounted } from "vue";
import ToDoItem from "./components/ToDoItem.vue";

const newTask = ref<string>("");

onMounted(() => {
  const savedTasks = localStorage.getItem("tasks");
  if (savedTasks) {
    array.value = JSON.parse(savedTasks);
  }
});

const array = ref([
  { id: 1, nome: "Task 1", done: false },
  { id: 2, nome: "Task 2", done: false },
  { id: 3, nome: "Task 3", done: false },
]);

const addTask = () => {
  if (newTask.value.trim() === "") {
    alert("Select a valid task name");
    return;
  }

  array.value.push({
    id: array.value.length + 1,
    nome: newTask.value,
    done: false,
  });
  newTask.value = "";
};

watchEffect(() => {
  localStorage.setItem("tasks", JSON.stringify(array.value));
});

const toggleTask = (index: number) => {
  array.value[index].done = !array.value[index].done;
  console.log("toggleTask", index, array.value[index].done);
};
const deleteTask = (index: number) => {
  array.value.splice(index, 1);
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
.wrapped {
  background-color: white;
  padding: 4rem;
  border-radius: 10px;
}
.input-task {
  margin: 0.5rem 0.5rem 0.5rem 0rem;
  padding: 0.5rem;
  width: 24rem;
  border-radius: 5px;
  border: 1px solid #3f3f46;
}
.btn {
  cursor: pointer;
  color: white;
}
.add {
  background-color: #16a34a;
  border-radius: 5px;
  border: 1px solid #3f3f46;
  padding: 0.5rem 1rem;
}
.add:hover {
  background-color: #166534;
}
.delete {
  background-color: #dc2626;
  border-radius: 10px;
  border: 1px solid black;
  width: 4rem;
  height: 3rem;
}
.delete:hover {
  background-color: #991b1b;
}
.trash{
  width: 1.8rem;
  height: 1.8rem;
}

.empty {
  margin-top: 1rem;
  font-size: 1.2rem;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 1rem 0;
}

.task-info {
  flex-grow: 1;
  margin-right: 2rem;
}

.task-list {
  max-height: 400px;
  overflow-y: auto;
  scrollbar-width: none;
}
</style>
