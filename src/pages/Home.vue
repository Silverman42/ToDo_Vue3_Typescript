<script setup lang="ts">
import { reactive, onMounted, ref, computed } from "vue";
interface Task {
  name: string;
  description: string;
  isDone: true | false;
}
const taskList: Task[] = reactive([]);
const taskName = ref<string>("");
const taskDescription = ref<string>("");

const createTask = (): void => {
  taskList.push({
    name: taskName.value,
    description: taskDescription.value,
    isDone: false,
  });
  resetTaskValues();
};

const resetTaskValues = (): void => {
  taskName.value = "";
  taskDescription.value = "";
};

const removeTask = (taskIndex: number): void => {
  taskList.splice(taskIndex, 1);
};

const markAsDone = (taskIndex: number): void => {
  taskList[taskIndex].isDone = true;
};

const buttonDisabled = computed<boolean>(() => {
  return taskName.value.length === 0 || taskDescription.value.length === 0;
});

onMounted(() => {
  window.document.querySelector("body")?.classList.add("bg-slate-700");
});
</script>
<template>
  <section class="bg-slate-700 py-10 min-h-screen px-3">
    <div
      class="max-w-lg rounded-lg mb-10 bg-slate-800 pt-8 pb-10 px-6 mx-auto border-slate-900 border"
    >
      <h1 class="text-lg mb-5 text-white">Create task</h1>
      <form @submit.prevent="createTask">
        <div class="mb-5">
          <label for="task_name" class="text-sm text-slate-400 mb-2 block"
            >Task name</label
          >
          <input
            type="text"
            v-model="taskName"
            class="block w-full border-slate-900 bg-slate-900 rounded-lg px-4 py-2 focus:ring-1 hover:ring-1 outline-none text-white ring-blue-5s00"
          />
        </div>
        <div class="mb-5">
          <label for="task_name" class="text-sm text-slate-400 mb-2 block"
            >Description</label
          >
          <textarea
            rows="5"
            v-model="taskDescription"
            class="block w-full border-slate-900 bg-slate-900 rounded-lg px-4 py-2 focus:ring-1 hover:ring-1 outline-none text-white ring-blue-5s00"
          ></textarea>
        </div>
        <div class="flex justify-end">
          <button
            :disabled="buttonDisabled"
            type="submit"
            class="py-3 px-6 text-sm disabled:bg-blue-900 bg-blue-700 hover:bg-blue-800 focus:bg-blue-800 rounded-xl text-blue-200"
          >
            Add task
          </button>
        </div>
      </form>
    </div>
    <div
      class="max-w-lg rounded-lg pt-10 bg-slate-800 py-4 px-6 mx-auto"
      v-if="taskList.length > 0"
    >
      <h1 class="text-lg mb-10 text-white">Task list</h1>
      <div class="mb-10" v-for="(task, index) in taskList" :key="index">
        <div class="w-full">
          <div class="flex mb-1 justify-between w-full">
            <p class="text-white">{{ task.name }}</p>
            <span
              v-if="task.isDone"
              class="px-3 py-1 inline-block bg-blue-900 text-blue-600 border rounded-full text-xs border-blue-600"
            >
              Completed
            </span>
            <span
              v-else
              class="px-3 py-1 inline-block bg-slate-700 border rounded-full text-slate-500 text-xs border-slate-500"
            >
              In progress
            </span>
          </div>
          <small class="text-slate-400 block">{{ task.description }}</small>
          <div class="flex gap-x-2 justify-end mt-3 w-full">
            <button
              @click="removeTask(index)"
              class="py-3 px-6 text-sm bg-red-700 hover:bg-red-800 focus:bg-red-800 rounded-xl text-red-200"
            >
              Remove task
            </button>
            <button
              v-if="!task.isDone"
              @click="markAsDone(index)"
              class="py-3 px-6 text-sm bg-blue-700 hover:bg-blue-800 focus:bg-blue-800 rounded-xl text-blue-200"
            >
              Complete task
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<style></style>
