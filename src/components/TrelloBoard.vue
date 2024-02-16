<script setup lang="ts">
import type {Column, Task} from "@/types";
import TrelloBoardTask from "@/components/TrelloBoardTask.vue";
import Draggable from 'vuedraggable';
import DragHandle from "@/components/DragHandle.vue";
import {useLocalStorage} from "@vueuse/core";

const options = {
  serializer: {
    read: (value) => {
      return JSON.parse(value).map((column: Column) => {
        if (column.tasks == null) {
          return column;
        }

        column.tasks = column.tasks.map((task: Task) => {
          task.created_at = new Date(task.created_at);
          return task;
        });
        return column;
      });
    },
    write: (value: string) => JSON.stringify(value),
  },
};

const columns = useLocalStorage<Array<Column>>("trelloBoard", [
  {
    id: 1,
    title: 'To Do',
    tasks: [
        {
          id: 1,
          title: 'Create an awesome app',
          created_at: new Date(),
        },
      {
          id: 2,
          title: 'Learn new things',
          created_at: new Date(),
        },
      {
          id: 3,
          title: 'Write a blog post',
          created_at: new Date(),
        },
    ]
  },
  {
    id: 2,
    title: 'In progress',
    tasks: [

    ]
  },
  {
    id: 3,
    title: 'Done',
    tasks: [

    ]
  }
]);
</script>

<template>
  <h1 class="text-2xl font-bold text-gray-700 mb-10">
    Trello Jutsu 🥷
  </h1>

  <div>
    <Draggable
        v-model="columns"
        group="columns"
        :animation="150"
        handle=".drag-handle"
        item-key="id"
        class="flex gap-4 overflow-x-auto items-start"
    >
      <template #item="{element: column}: {element: Column}">
        <div class="bg-gray-50 p-5 rounded shadow-lg min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle icon="🖲️" />

            <input
              class="bg-transparent focus:bg-white rounded px-1 w-4/5"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              type="text"
              v-model="column.title"
            />
          </header>

          <Draggable
              v-model="column.tasks"
              group="tasks"
              :animation="150"
              handle=".drag-handle"
              item-key="id"
          >
            <template
                #item="{element: task}: {element: Task}"
            >
              <TrelloBoardTask
                  :task="task"
              />
            </template>
          </Draggable>

        </div>
      </template>
    </Draggable>

  </div>
</template>