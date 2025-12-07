<script setup lang="ts">
const props = defineProps({
  class: {
    type: String,
  },
  layoutClass: {
    type: String,
  },
});
</script>

<template>
  <div class="slidev-layout two-columns w-full h-full grid grid-cols-3"
       :class="props.layoutClass"
  >
    <div class="div col-span-3">
      <slot name="title"/>
    </div>

    <div class="col-left col-span-1" :class="props.class">
      <slot name="left"/>
    </div>

    <div class="col-right col-span-2" :class="props.class">
      <slot name="right"/>
    </div>
  </div>
</template>
