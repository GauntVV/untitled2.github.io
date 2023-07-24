<template>
  <div
      class="flex w-full w-300 sm:w-auto bg-gray-800 text-white rounded-lg  items-center justify-center px-4 py-2.5 dark:bg-gray-700 dark:hover:bg-gray-600 dark:focus:ring-gray-700"
  >
    <Listbox as="div" v-model="selectedItem"  class=" w-72">
      <ListboxLabel class="block w-full text-sm font-medium leading-6"
      >Name</ListboxLabel
      >
      <div class="relative mt-2 ">
        <ListboxButton
            class="relative block w-full  cursor-default rounded-md bg-white py-1.5 pl-3 pr-10 text-left text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 sm:text-sm sm:leading-6"
        >
          <span class="flex items-center">
            <span class="ml-3 block w-full">{{ selectedItem.name }}</span>
          </span>
          <span
              class="pointer-events-none absolute inset-y-0 right-0 ml-3 flex items-center pr-2"
          >
            <ChevronUpDownIcon
                class="h-5 w-5 text-gray-400"
                aria-hidden="true"
            />
          </span>
        </ListboxButton>

        <transition
            leave-active-class="transition ease-in duration-100"
            leave-from-class="opacity-100"
            leave-to-class="opacity-0"
        >
          <ListboxOptions
              class="absolute z-10 mt-1 max-h-56 block w-full overflow-auto rounded-md bg-white py-1 text-base shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none sm:text-sm"
          >
            <ListboxOption
                as="template"
                v-for="item in data"
                :key="item.name"
                :value="item"
                v-slot="{ active, selectedItem }"
            >
              <li
                  :class="[
                  active ? 'bg-indigo-600 text-white' : 'text-gray-900',
                  'relative cursor-default select-none py-2 pl-3 pr-9',
                ]"
              >
                <div class="flex w-full items-center">
                  <span
                      :class="[
                      selectedItem ? 'font-semibold' : 'font-normal',
                      'ml-3 block ',
                    ]"
                  >{{ item.name }}</span
                  >
                </div>
              </li>
            </ListboxOption>
          </ListboxOptions>
        </transition>
      </div>
    </Listbox>
  </div>
</template>

<script setup>
import { computed } from "vue";
import {
  Listbox,
  ListboxButton,
  ListboxLabel,
  ListboxOption,
  ListboxOptions,
} from "@headlessui/vue";
import { ChevronUpDownIcon } from "@heroicons/vue/20/solid";

const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
  selected: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["update:selected"]);

const selectedItem = computed({
  get() {
    return props.selected;
  },
  set(value) {
    emit("update:selected", value);
  },
});
</script>