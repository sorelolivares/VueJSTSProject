<script setup lang="ts">
import {ref} from 'vue'
import NumberCounter from './components/NumberCounter.vue'
import TaskManager from './components/TaskManager.vue';
import GithubRepositoryFinder from './components/GithubRepositoryFinder.vue';
const counterComponent = ref(false)
const taskManagerComponent = ref(false)
const githubRepositoryFinderComponent = ref(false)
</script>
<template>
  <div class="p-6 bg-gray-100 min-h-screen min-w-screen">
    <div class="w-full">
      <div class="w-full mb-4">
        <button @click="counterComponent = !counterComponent" class="py-1 px-3 rounded-lg bg-emerald-800 text-white ">CONTADOR</button>
        <NumberCounter v-if="counterComponent"/>
      </div>
      <div class="w-full mb-4">
        <button @click="taskManagerComponent = !taskManagerComponent" class="py-1 px-3 rounded-lg bg-emerald-800 text-white ">TASK MANAGER</button>
        <TaskManager v-if="taskManagerComponent"/>
      </div>
      <div class="w-full mb-4">
        <button @click="githubRepositoryFinderComponent = !githubRepositoryFinderComponent" class="py-1 px-3 rounded-lg bg-emerald-800 text-white ">Github Random Repository Finder</button>
        <GithubRepositoryFinder v-if="githubRepositoryFinderComponent"/>
      </div>
    </div>
  </div>
</template>
