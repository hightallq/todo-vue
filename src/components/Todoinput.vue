<script setup>
import { ref } from 'vue';

const emit = defineEmits(['send-text']);
const inputText = ref('');

function addTask() {
    if (inputText.value === '') {
        alert('Task cannot be empty');
        return;
    }
    if (inputText.value.length > 20) {
        alert('Task is too long');
        return;
    }
    emit('send-text', inputText.value);
    inputText.value = '';
}
</script>

<template>
    <div class="todo_input">
        <input class="input_text" v-model="inputText" type="text" placeholder="Add a new task"
            @keyup.enter="addTask()" />
        <button class="add_button" @click="addTask()">Add Task</button>
    </div>
</template>

<style scoped>
.todo_input {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
}

.input_text {
    width: 70%;
    padding: 10px;
    margin-right: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
    transition: all 0.3s ease;
}

.add_button {
    width: 100px;
    padding: 10px;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s ease;
}

.input_text:focus {
    outline: none;
    border-color: #007bff;
}

.add_button:hover {
    background-color: #029121;
}
</style>
