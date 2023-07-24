<template>
  <div
      class="w-full sm:w-auto bg-gray-800 text-white rounded-lg inline-flex items-center justify-center px-4 py-2.5 dark:bg-gray-700 dark:hover:bg-gray-600 dark:focus:ring-gray-700"
  >
    <div>
      <label for="price" class="block text-sm font-medium leading-6"
      >Value</label
      >
      <div class="relative mt-2 rounded-md shadow-sm">
        <input
            v-model="modelValue.value"
            type="text"
            name="price"
            id="price"
            class="block w-full w-72 rounded-md border-0 py-1.5 pl-7 pr-7 text-gray-900 ring-1 ring-inset ring-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 sm:text-sm sm:leading-6"
            placeholder="value"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  modelValue: {
    type: Object,
  },
});
</script>