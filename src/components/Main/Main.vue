<script setup>
import { ref, nextTick } from 'vue';
import draggable from 'vuedraggable';

const tasks = ref([]);
const task = ref('');

const handleInput = (e) => {
    task.value = e.target.value;
}

const addTask = (e) => {
    e.preventDefault();
    if (task.value != '') {
        tasks.value.push(task.value);
        task.value = '';
        document.getElementById("inputText").value = '';

        const taskContainer = document.querySelector('.taskList');
        nextTick(() => {
            if (taskContainer) {
                taskContainer.scrollTop = taskContainer.scrollHeight;
            }
        });
    }
}

const removeTask = (event) => {
    const index = event.target.getAttribute('data-index');
    tasks.value.splice(index, 1);
}
</script>

<template>
    <main>
        <h1 class="mt-20 font-semibold">To Do List</h1>

        <form class="mt-26 flex flex-row gap-4" @submit="addTask">
            <input type="text" id="inputText" name="inputText" placeholder="Enter a task"
                class="w-68 px-4 py-2 bg-white text-[#242424] rounded-md" @input="handleInput">
            <button>Add</button>
        </form>

        <draggable class="taskList mt-20 max-h-56 overflow-auto flex flex-col gap-4" v-model="tasks" item-key="index"
            animation="200" ghost-class="ghost" chosen-class="chosen" drag-class="dragging">
            <template #item="{ element, index }">
                <div
                    class="p-2 flex flex-row justify-between bg-white text-[#242424] rounded-md shadow-md transition-all duration-300 hover:opacity-80">
                    <p class="max-w-72 overflow-x-auto whitespace-nowrap">{{ element }}</p>
                    <div class="w-7 h-7 bg-red-500 rounded-full text-white cursor-pointer transition duration-300 hover:bg-red-600"
                        @click="removeTask" :data-index="index">-</div>
                </div>
            </template>
        </draggable>
    </main>
</template>

<style scoped>
.ghost {
    opacity: 0.5;
}

.chosen {
    background-color: #e2e8f0;
}

.dragging {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
</style>