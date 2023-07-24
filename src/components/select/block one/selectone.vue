<template>
  <div class="
$(document).ready(function(){
$(".dropdown").click(function() {
$(".menu").toggleClass("showMenu");
$(".menu > li").click(function() {
$(".dropdown > p").html($(this).html());
$(".menu").removeClass("showMenu");
});
});
})">
  </div>
</template>
<script setup>
import { computed } from "vue";
import {
  .dropdown,
  .menu,
  showMenu,
  } from "@headlessui/vue";
import { ChevronUpDownIcon } from "@heroicons/vue/20/solid";

const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
  selectone: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["updates:selectone"]);

const selectone = computed({
  get() {
    return props.select;
  },
  set(value) {
    emit("update:selectone", value);
  },
});
</script>