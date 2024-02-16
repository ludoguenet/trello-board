<script setup lang="ts">
import type {Task} from "@/types";
import DragHandle from "@/components/DragHandle.vue";

defineProps<{
  task: Task,
}>();
</script>

<template>
<div
    :title="new Date(task.created_at).toLocaleDateString()"
    class="bg-white p-2 mb-2 rounded shadow-sm max-w-[275px]"
>
<DragHandle icon="⚫" class="mr-1" />
<span>
  <input
    class="bg-transparent focus:bg-white rounded px-1 w-4/5"
    @keyup.enter="($event.target as HTMLInputElement).blur()"
    v-model="task.title"
  />
</span>
</div>
</template>

<style scoped>

</style>