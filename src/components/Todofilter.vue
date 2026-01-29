<script setup>
const emit = defineEmits(['filter-tasks']);

function filterTasks(button) {
    if (button == 'completed') {
        emit('filter-tasks', 'completed');
    }
    if (button == 'uncompleted') {
        emit('filter-tasks', 'uncompleted');
    }
    if (button == 'all') {
        emit('filter-tasks', 'all');
    }
}
</script>

<template>
    <div class="button_div">
        <button class="filter_button" @click="filterTasks('completed')">
            Completed
        </button>
        <button class="filter_button" @click="filterTasks('uncompleted')">
            Uncompleted
        </button>
        <button class="filter_button" @click="filterTasks('all')">
            Show All
        </button>
    </div>
</template>

<style scoped>
.button_div {
    display: flex;
    justify-content: center;
}

.filter_button {
    padding: 10px;
    margin: 0 5px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s ease;
}

.filter_button:hover {
    background-color: #0056b3;
}

@media screen and (max-width: 500px) {
    .button_div {
        flex-direction: row;
        align-items: center;
        flex-wrap: wrap;
        height: 35%;
    }
    .filter_button {
        width: 40%;
        margin-bottom: 5px;
        padding: 8px;
        font-size: 14px;
        margin: 0 3px;
    }

}
</style>
