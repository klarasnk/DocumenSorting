<template>
  <div class="w-full flex justify-end">
    <div
      class="h-[50px] border flex items-center justify-between px-[15px] cursor-pointer"
      :style="{ width: width }"
    >
      <div class="flex gap-[15px] items-center">
        <div class="text-[15px]">
          <slot name="documentType"></slot>
        </div>
        <div class="flex gap-[10px]">
          <slot name="coloredDot"></slot>
        </div>
        <div class="text-[12px] text-[#ff238d]">
          <slot name="mandatory"></slot>
        </div>
        <div class="text-[#b1bbd0] text-[12px]">
          <slot name="documentDetails"></slot>
        </div>
      </div>

      <DocumentChanges
        :documentId="documentId"
      />
    </div>
  </div>
</template>

<script setup>
import DocumentChanges from "./DocumentChanges.vue";

const props = defineProps({
  width: String,
  documentId: Number,
  
});
</script>
