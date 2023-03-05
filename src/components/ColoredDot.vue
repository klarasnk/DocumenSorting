<template>
  <div class="w-[10px] h-[10px] rounded-full " :style="{ 'background-color': dotColor }"></div>
</template>
<script setup>
const props= defineProps({
    dotColor:String
})
</script>
