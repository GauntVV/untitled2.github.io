<template>
  <div class="dropholder">
<div>
<p>Select</p>
<div class="dropdown">
  <p><i class="fa fa-foursquare"></i> Please Rate</p>
</div>
<ul class="menu">
  <li><i class="fa fa-thumbs-down"></i> Ugly</li>
  <li><i class="fa fa-thumbs-up"></i> Not bad</li>
  <li><i class="fa fa-bolt"></i> Ok it's good</li>
  <li><i class="fa fa-star"></i> Very Nice works</li>
  <li><i class="fa fa-heart"></i> Wow how you did it</li>
</ul>
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