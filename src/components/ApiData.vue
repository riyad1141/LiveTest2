<script setup>

import {ref} from "vue";

const tasks = ref([
  { name: "Task 1", time: 30 },
  { name: "Task 2", time: 40 },
  { name: "Task 3", time: 60 },
  { name: "Task 4", time: 45 },
  { name: "Task 5", time: 50 },
]);

const showEditPopup = ref(false);
const editedTask = ref({ name: "", time: "" });
const editedTaskIndex = ref(-1);

const editTask = (index) => {
  editedTask.value = { ...tasks.value[index] };
  editedTaskIndex.value = index;
  showEditPopup.value = true;
};

const updateTask = () => {
  if (editedTaskIndex.value !== -1) {
    tasks.value[editedTaskIndex.value] = { ...editedTask.value };
    showEditPopup.value = false;
  }
};

const newTask = ref({
  name: "",
  time: "",
});

const showAddPopup = ref(false);

const hideAddPopup = () => {
  // Clear input fields when hiding the popup
  newTask.value.name = "";
  newTask.value.time = "";
  // Update the reactive variable directly
  showAddPopup.value = false;
};

const addTask = () => {
  tasks.value.push({
    name: newTask.value.name,
    time: newTask.value.time,
  });

  // Hide the Add Task popup
  hideAddPopup();
};

</script>

<template>

  <div class="container mt-5">
    <div class="card">
      <div class="card-header bg-primary text-white">
        <h4>Task List</h4>
      </div>
      <div class="card-body">
        <div v-for="(task, index) in tasks" :key="index" class="mb-4">
          <div class="d-flex justify-content-between align-items-center">
            <div class="mb-3">
              <h5>Task Name:</h5>
              <p>{{ task.name }}</p>
              <h5>Time:</h5>
              <p>{{ task.time }} minutes</p>
            </div>
            <button class="btn btn-primary" @click="editTask(index)">Edit</button>
          </div>
          <hr class="my-4">
        </div>
      </div>
      <button class="btn btn-success mt-3" @click="showAddPopup = true">Add New Task</button>
    </div>

    <!-- Edit Task Popup -->
    <div v-if="showEditPopup" class="edit-popup">
      <div class="edit-popup-content bg-white p-4 rounded">
        <label for="editTaskName">Task Name:</label>
        <input type="text" id="editTaskName" class="form-control" v-model="editedTask.name">

        <label for="editTaskTime">Time:</label>
        <input type="text" id="editTaskTime" class="form-control" v-model="editedTask.time">

        <button class="btn btn-success mt-3" @click="updateTask">Update</button>
      </div>
    </div>

    <!-- Add Task Popup -->
    <div v-if="showAddPopup" class="edit-popup">
      <div class="edit-popup-content bg-white p-4 rounded">
        <label for="addTaskName">Task Name:</label>
        <input type="text" id="addTaskName" class="form-control" v-model="newTask.name">

        <label for="addTaskTime">Time:</label>
        <input type="text" id="addTaskTime" class="form-control" v-model="newTask.time">

        <button class="btn btn-success mt-3" @click="addTask">Add</button>
        <button class="btn btn-secondary mt-3 ml-2" @click="hideAddPopup">Cancel</button>
      </div>
    </div>
  </div>

</template>

<style scoped>

.edit-popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

body {
  background-color: #f8f9fa;
}

.card {
  border: 1px solid #ccc;
  border-radius: 10px;
  overflow: hidden;
}

.card-body {
  padding: 20px;
}

.btn-primary {
  background-color: #007bff;
  border-color: #007bff;
}

.btn-primary:hover {
  background-color: #0056b3;
  border-color: #0056b3;
}

.btn-success {
  background-color: #28a745;
  border-color: #28a745;
}

.btn-success:hover {
  background-color: #218838;
  border-color: #218838;
}

</style>