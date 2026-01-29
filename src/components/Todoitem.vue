<script setup>
const props = defineProps({
    task: Object,
    isSelected: Boolean
});
const emit = defineEmits(['delete-task', 'toggle-completion', 'swap-task']);

function deleteTask(taskId) {
    emit('delete-task', taskId);
}

function toggleTask(taskId) {
    emit('toggle-completion', taskId);
}

function swapTask(taskId) {
    emit('swap-task', taskId);
}
</script>

<template>
    <div @click="swapTask(task.id)" class="task" :class="{ selected: isSelected }, { completed: task.completed }">
        <input @click="toggleTask(task.id)" type="checkbox" v-bind:checked="task.completed" />
        <span>{{ task.text }}</span>
        <button @click="deleteTask(task.id)">X</button>
    </div>
</template>

<style scoped>
.task {
    background-color: #f0f0f0;
    width: 280px;
    height: 60px;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
    margin-right: 10px;
    padding: 0 10px;
    border-radius: 5px;
    transition: all 0.3s ease;
}

.task:hover {
    background-color: #e0e0e0;
}

.task input {
    width: 25px;
    height: 25px;
    cursor: pointer;
}

.task button {
    background-color: red;
    color: #e0e0e0;
    border: none;
    border-radius: 3px;
    cursor: pointer;
    width: 25px;
    height: 25px;
    font-weight: bold;
    font-size: 1rem;
    transition: all 0.3s ease;
}

.task span {
    cursor: default;
}

.task.selected {
    animation: selectedAnimation 1.5s infinite;
}

.task.completed {
    opacity: 0.6;
}

.task button:hover {
    background-color: #cb0000;
}

@media screen and (max-width: 500px) {
    .task {
        width: 220px;
        height: 50px;
    }
}

@keyframes selectedAnimation {
    0% {
        background-color: #cfcfcf;
    }

    50% {
        background-color: #f0f0f0;
        transform: scale(0.98);
    }
    
    100% {
        background-color: #cfcfcf;
    }
}
</style>
