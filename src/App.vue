<script setup>
import Todoinput from './components/Todoinput.vue';
import Todoitem from './components/Todoitem.vue';
import Todofilter from './components/Todofilter.vue';
import { ref, onMounted, computed, watch } from 'vue';

const tasks = ref([
  {
    order: 1,
    id: 1,
    text: 'Learn Vue.js',
    completed: false,
  },
  {
    order: 2,
    id: 2,
    text: 'Build a todo app',
    completed: false,
  },
  {
    order: 3,
    id: 3,
    text: 'Read Vue documentation',
    completed: true,
  }
]);

const taskText = ref('');

watch(tasks, (newTasks) => {
  localStorage.setItem('tasks', JSON.stringify(newTasks));
}, { deep: true });

function addTask(newTask) {
  taskText.value = newTask;
  tasks.value.push({
    order: tasks.value.length + 1,
    id: Date.now(),
    text: taskText.value,
    completed: false,
  });
}

function deleteTask(taskId) {
  tasks.value = tasks.value.filter((task) => task.id !== taskId);
}

function toggleTaskCompletion(taskId) {
  tasks.value = tasks.value.map(task => {
    if (task.id === taskId) {
      return {
        ...task, completed: !task.completed
      }
    }
    return task;
  });
}

let selectedTaskId = ref(null);
function swapTask(targetedTaskId) {
  if (selectedTaskId.value === null) {
    selectedTaskId.value = targetedTaskId;
    return;
  }
  const taskIndex1 = tasks.value.findIndex(task => task.id === selectedTaskId.value);
  const taskIndex2 = tasks.value.findIndex(task => task.id === targetedTaskId);
  if (taskIndex1 === taskIndex2) {
    selectedTaskId.value = null;
    return;
  }
  const tempOrder = tasks.value[taskIndex1].order;
  tasks.value[taskIndex1].order = tasks.value[taskIndex2].order;
  tasks.value[taskIndex2].order = tempOrder;
  tasks.value.sort((a, b) => a.order - b.order);
  selectedTaskId.value = null;
}

function unSwapTask() {
  if (selectedTaskId.value !== null) selectedTaskId.value = null;
  return;
}

const filterButton = ref('');
function filterTasks(button) {
  filterButton.value = button;
}

const visibleTasks = computed(() => {
  const activeTasks = tasks.value.filter(task => !task.completed);
  const completedTasks = tasks.value.filter(task => task.completed);;
  if (filterButton.value === 'completed') {
    return tasks.value.filter(task => task.completed);
  } else if (filterButton.value === 'uncompleted') {
    return tasks.value.filter(task => !task.completed);
  } else {
    return [...activeTasks, ...completedTasks];
  }
});

onMounted(() => {
  const savedTasks = localStorage.getItem('tasks');
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
});
</script>

<template>
  <div class="app" tabindex="0" @keyup.esc="unSwapTask">
    <div class="border_base">
      <div class="header">
        <h1>Todo-list on <span>Vue3</span></h1>
        <p>Please, add your tasks to the list.</p>
        <Todoinput @send-text="addTask" />
        <Todofilter @filter-tasks="filterTasks" />
      </div>
      <div class="tasks">
        <Todoitem v-for="task in visibleTasks" :key="task.id" :task="task" @swap-task="swapTask"
          @toggle-completion="toggleTaskCompletion" @delete-task="deleteTask" :list="visibleTasks"
          :isSelected="task.id === selectedTaskId" />
        <p v-if="visibleTasks.length === 0">No tasks yet...</p>
      </div>
    </div>
    <div class="sticky_note">
      <ul>
        <li>Press <span>'Enter'</span> to add a task.</li>
        <li>Click on a task to select it, then click another task to swap their positions.</li>
        <li>Press <span>'Esc'</span> to deselect a selected task.</li>
      </ul>
    </div>
    <div class="author">
      <p>© Created by amigoboy - 2026</p>
    </div>
  </div>
</template>

<style scoped>

.border_base {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 2px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  width: 400px;
  background-color: #f9f9f9;
}

.header {
  text-align: center;
  margin-bottom: 20px;
}

.header h1 span {
  color: #42b983;
}

.tasks {
  max-height: 250px;
  overflow-y: auto;
}

.sticky_note {
  position: fixed;
  bottom: 25px;
  right: 25px;
  background-color: #fffa87;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  width: 200px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  transition: all 0.3s ease;
}

.sticky_note:hover {
  transform: rotate(5deg);
}

.sticky_note ul {
  padding-left: 20px;
}

.sticky_note li {
  margin-bottom: 10px;
}

.sticky_note span {
  font-weight: 800;
  color: red;
}

.author {
  position: fixed;
  bottom: 5px;
  left: 20px;
  margin-top: 15px;
  font-size: 14px;
  color: white;
}

@media screen and (max-width: 500px) {
  .border_base {
    width: 80%;
  }

  .header h1 {
    font-size: 1.5rem;
  }

  .sticky_note {
    display: none;
  }
}
</style>
