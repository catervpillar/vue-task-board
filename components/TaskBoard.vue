<script setup lang="ts">
import { nanoid } from 'nanoid'
import type { Column, ID, Task } from '@/types'
import draggable from "vuedraggable";
import TaskItem from '~/components/TaskItem.vue';

const columns = ref<Column[]>([
  {
    id: nanoid(),
    title: "ToDo 📋",
    tasks: [
      {
        id: nanoid(),
        title: "Create marketing landing page",
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: "Implement API",
        createdAt: new Date(),
      },
    ],
  },
  { id: nanoid(), title: "In Progress ⏳", tasks: [] },
  { id: nanoid(), title: "Completed ✅", tasks: [] },
]);

const alt = useKeyModifier("Alt");

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: "",
    tasks: []
  };
  columns.value.push(column);
  nextTick(() => {
    (document.querySelector(".column:last-of-type .title-input") as HTMLInputElement).focus();
  });
}

function deleteTask(columnId: ID, taskId: ID) {
  const column = columns.value.find(c => c.id === columnId);
  if (column != null) {
    column.tasks = column.tasks.filter(t => t.id !== taskId);
  }
}
</script>

<template>
  <div class="flex items-start overflow-x-auto gap-4">
    <draggable v-model="columns" group="columns" :animation="150" handle=".drag-handle" item-key="id"
      class="flex gap-4 items-start">
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle />
            <input type="text" placeholder="New Column"
              class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              @keydown.backspace="column.title === '' ? columns = columns.filter(c => c.id !== column.id) : null"
              v-model="column.title">
          </header>
          <draggable v-model="column.tasks" :group="{ name: 'tasks', pull: alt ? 'clone' : true }" :animation="150"
            handle=".drag-handle" item-key="id">
            <template #item="{ element: task }: { element: Task }">
              <div>
                <TaskItem :task="task" @delete="deleteTask(column.id, $event)" />
              </div>
            </template>
          </draggable>
          <footer>
            <NewTask @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>

    <button @click="createColumn" class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50">+ Add Another
      Column</button>
  </div>
</template>


<style scoped></style>