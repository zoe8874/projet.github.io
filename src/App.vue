<script setup>
import { ref, computed } from 'vue'


const tasks = ref([
  { text: 'Rasen mähen',        done: false, high_priority: false },
  { text: 'Französisch lernen', done: false, high_priority: true },
  { text: 'Einkaufen',          done: false, high_priority: true },
  { text: 'Abfall rausbringen', done: false, high_priority: true },
])


const newTaskText = ref('')
const newTaskPriority = ref(false)


const doneCount = computed(() => tasks.value.filter(task => task.done).length)
const totalCount = computed(() => tasks.value.length)


const sortedTasks = computed(() => {
  return [...tasks.value].sort((a, b) => b.high_priority - a.high_priority)
})


function addTask() {
  if (newTaskText.value.trim() === '') return

  tasks.value.push({
    text: newTaskText.value,
    done: false,
    high_priority: newTaskPriority.value,
  })


  newTaskText.value = ''
  newTaskPriority.value = false
}

function deleteTask(index) {
  tasks.value.splice(index, 1)
}
</script>

<template>
  <div class="container">
    <h2>Aufgabenliste</h2>

    <p>
      {{ doneCount }} von {{ totalCount }} Tasks sind erledigt
    </p>


    <div class="form-group">
      <label for="task">Neuer Task</label>
      <input
          class="form-control"
          id="task"
          type="text"
          v-model="newTaskText"
      />
    </div>

    <label for="prio">
      <input
          id="prio"
          type="checkbox"
          v-model="newTaskPriority"
      />
      Hohe Priorität
    </label>

    <div class="form-actions">
      <button @click="addTask">
        Task hinzufügen
      </button>
    </div>


    <ul>
      <li
          v-for="(task, index) in sortedTasks"
          :key="index"
          :class="[{ 'is-done': task.done }, { 'high-priority': task.high_priority }]"
      >
        <input
            type="checkbox"
            v-model="task.done"
        />
        {{ task.text }}
        <button @click="deleteTask(index)" style="margin-left: 10px;">
          Löschen
        </button>
      </li>
    </ul>
  </div>
</template>


<style>
.is-done {
  text-decoration: line-through;
}

.high-priority {
  font-weight: bold;
  color: red;
}

.form-group {
  display: block;
}

.form-group label {
  display: block;
  margin-bottom: 2px;
}

.form-control {
  width: 100%;
  padding: 2px 5px;
  height: 32px;
  margin-bottom: 5px;
}

.form-actions {
  display: block;
  margin-top: 1rem;
  margin-bottom: 2rem;
}

.container {
  margin: 20px auto;
  max-width: 400px;
  width: 100%;
}
</style>