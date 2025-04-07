<template>
  <div class="todolist">
    <div class="todolist-card">
      <h1>Voici Votre To-Do-List</h1>
      <input class="todolist-input" type="text" v-model="newTask" @keyup.enter="AddTaskList"
        placeholder="Ajouter une tâche" />
      <div class="todolist-header" v-if="tasks.length > 0">
        <span class="header-label task">Tâche</span>
        <span class="header-label">Ajouté à</span>
        <span class="header-label">Actions</span>
      </div>
      <transition-group name="fade" tag="ul" class="todolist-items" v-if="tasks.length > 0">
        <li v-for="(task, index) in tasks" :key="task.id">
          <div class="todolist-task">
            <input type="checkbox" v-model="task.done" @click="toggleTaskDone(index)" />
            <span :class="{ 'done': task.done }"><strong>{{ task.name }}</strong></span>
          </div>
          <span :class="{ 'done': task.done }">{{ task.date }}</span>
          <button class="todolist-deleted-button" @click="DeleteTaskList(index)">Supprimer</button>
        </li>
      </transition-group>
      <div class="todolist-alldeleted-button" v-show="tasks.length > 1">
        <div>
          <p>Nombre de tâche(s) créée(s) : <strong :class="{ 'full': taskFull }">{{ tasks.length }}</strong></p>
        </div>
        <hr>
        <div>
          <button class="todolist-deleted-button" @click="AllDeleteTaskList()">Supprimer toutes les tâches</button>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup lang="ts">

import { reactive, ref } from "vue";

const newTask: string = ref("");

const tasks: Array<{ id: number, name: string, done: boolean, date: string }> = reactive([]);

const taskDone: boolean = ref(false);
const taskFull: boolean = ref(false);

function AddTaskList() {
  if (newTask.value.trim() !== "" && tasks.length < 30) {
    const dateNow = new Date().toLocaleString("fr-FR", {
      year: "numeric",
      month: "long",
      day: "numeric",
      hour: "2-digit",
      minute: "2-digit",
    });

    tasks.push({
      id: tasks.length + 1,
      name: newTask.value,
      done: taskDone.value,
      date: dateNow,
    });

    newTask.value = "";
    TaskItemFull();
  }
}


function DeleteTaskList(index: number) {
  if (index >= 0 && index < tasks.length) {
    tasks.splice(index, 1);
    TaskItemFull();
  } else {
    console.error("Index out of bounds");
  }
}
function AllDeleteTaskList() {
  tasks.splice(0, tasks.length);
  console.log(tasks);
  newTask.value = "";
  taskDone.value = false;
  taskFull.value = false;
}

function toggleTaskDone(index: number) {
  if (index >= 0 && index < tasks.length) {
    tasks[index].done = !tasks[index].done;
  } else {
    console.error("Index out of bounds");
  }
}

function TaskItemFull() {
  if (tasks.length >= 30) {
    taskFull.value = true;
  } else {
    taskFull.value = false;
  }
}
</script>

<style>
@media (min-width: 1024px) {
  .todolist {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .todolist-card {
    width: 100%;
    max-width: 600px;
    padding: 2rem;
    border-radius: 1rem;
    background-color: #fefefe;
    box-shadow: 0 0 10px rgba(255, 255, 255, 0.8);
    color: black;
  }

  .todolist-card h1 {
    text-align: center;
    margin-bottom: 2rem;
  }

  .todolist-input {
    width: 100%;
    padding: 1rem;
    border-radius: 0.5rem;
    border: 1px solid #ccc;
    margin-bottom: 2rem;
    font-size: 1rem;
  }

  .todolist-items {
    list-style: none;
    padding: 0;
    max-height: 300px;
    overflow-y: overlay;
  }

  .todolist-items li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem;
    border-bottom: 1px solid #ccc;
  }

  .todolist-items li span {
    padding: 1rem;
  }

  .todolist-alldeleted-button {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    margin-top: 2rem;
  }

  .todolist-deleted-button {
    background-color: #ff4d4d;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 0.5rem;
    cursor: pointer;
  }

  .todolist-deleted-button:hover {
    background-color: #ff1a1a;
  }

  .done {
    text-decoration: line-through;
    color: #ccc;
  }

  .todolist-header {
    display: flex;
    justify-content: space-between;
    font-weight: bold;
    background-color: #f0f0f0;
    border-bottom: 2px solid #bbb;
  }

  .header-label {
    padding: 1rem;
  }

  .header-label .task,
  .todolist-task {
    width: 250px;
  }

  /* Animation de mes tâches */

  .fade-enter-active,
  .fade-leave-active {
    transition: all 0.7s ease-in-out;
  }

  .fade-enter-from {
    opacity: 0;
    transform: translateX(20px);
  }

  .fade-leave-to {
    opacity: 0;
    transform: translateY(-10px);
  }

  .full {
    color: red;
  }

}
</style>
