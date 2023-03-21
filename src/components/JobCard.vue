<template>
  <div
    class="flex justify-between items-center bg-yellow-100 p-2 rounded-md shadow"
  >
    <p class="text-green-700">{{ jobTitle }}</p>
    <button
      @click="$emit('handleSelectedJob', $event)"
      class="text-white bg-orange-400 rounded-md py-1 px-3"
    >
      See Job
    </button>
  </div>
</template>

<script setup lang="ts">
interface Props {
  jobTitle: string;
}

defineProps<Props>();

defineEmits<{
  (e: "handleSelectedJob"): void;
}>();
</script>
