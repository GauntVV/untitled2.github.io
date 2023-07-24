<template>
  <div
      class="w-full h-screen flex justify-center p-4 text-center bg-white border border-gray-200 sm:p-8 dark:bg-gray-800 dark:border-gray-700"
  >
    <div
        class="items-center justify-center space-y-4 sm:flex sm:space-y-0 sm:space-x-4"
    >
      <MySelect
          :data="data"
          :selected="selected"
          @update:selected="selected = $event"
      />

      <MyTextInput v-model="selected" />

    </div>
  </div>
</template>

<script setup>
import MySelect from "./components/MySelect.vue";
import MyTextInput from "./components/MyTextInput.vue";
import { onMounted, ref } from "vue";

const data = ref([]);
const selected = ref({});

onMounted(() => {
  data.value = generateObjectArray();
  selected.value = data.value[0];
});

function generateObjectArray() {
  const minObjects = 20;
  const maxObjects = 30;
  const objectArray = [];
  const objectCount =
      Math.floor(Math.random() * (maxObjects - minObjects + 1)) + minObjects;

  for (let i = 1; i <= objectCount; i++) {
    const obj = {
      name: "foo" + i,
      value: "bar" + i,
    };

    objectArray.push(obj);
  }
  return objectArray;
}
</script>