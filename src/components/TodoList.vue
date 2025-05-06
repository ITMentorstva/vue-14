<template>
  <CreateTask
      @add-task="addTask"
      @close-create-task="closeCreateTaskPopup"
      v-if="showCreatePopup"
  ></CreateTask>

  <SortTask
    v-model:prioritySort="taskStore.prioritySort"
    @change-sort="changeSort"
  ></SortTask>

  <AllTasks
      :groupedTasks="taskStore.groupedTasks"
      @showCreatePopup="openCreatePopup"
  ></AllTasks>

</template>

<script setup lang="ts">

  import CreateTask from "@/components/tasks/createTask.vue";
  import SortTask from "@/components/tasks/sortTask.vue";
  import AllTasks from "@/components/tasks/allTasks.vue";
  import {useTaskStore} from "@/stores/useTaskStore";
  import {onMounted, ref, watch} from "vue";
  import TaskType from "@/Types/TaskType";

  const taskStore = useTaskStore();
  const showCreatePopup = ref(false);

  onMounted(() => {
    taskStore.loadAllTasks()
  });

  watch(
      () => taskStore.tasks,
      () => {
        taskStore.saveTasks()
      },
      { deep: true }
  )

  function openCreatePopup() {
    showCreatePopup.value = true;
  }

  function closeCreateTaskPopup() {
    showCreatePopup.value = false;
  }

  function changeSort() {
    taskStore.changeSort();
  }

  function addTask(newTask: TaskType) {
    taskStore.addTask(newTask);
  }

</script>