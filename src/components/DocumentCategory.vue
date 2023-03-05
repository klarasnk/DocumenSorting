<template>
  <div>
    <div v-if="category.documentCategory!== 'documentsWithoutCategories'"
      
      class="w-full h-[60px] border flex items-center justify-between px-[15px] cursor-pointer"
    >
      <div class="flex gap-[15px] items-center">
        <div
          class="w-[30px] h-[30px] rounded-full border flex items-center justify-center text-blue-700"
          :class="{ 'rotate-180': showDropdown }"
        >
          <span class="material-symbols-outlined">expand_more</span>
        </div>
        <div class="font-bold text-[17px]">
          <slot name="documentCategory"></slot>
        </div>
        <div class="flex gap-[10px]">
          <slot name="coloredDot"></slot>
        </div>
        <div class="text-[12px] text-[#b1bbd0]">
          <slot name="categoryDetails"></slot>
        </div>
      </div>
      <DocumentChanges 
        :categoryId="categoryId"
        :documentCategories="documentCategories"
        @deleteCategory="$emit('deleteCategory', categoryId)"
        :dragIcon="dragIcon"
      />
    </div>


    <slot name="documentType"></slot>
  </div>
</template>

<script setup>
import DocumentChanges from "@/components/DocumentChanges.vue";

const props = defineProps({
  documentCategories: Array,
  categoryId: Number,
  dragIcon:Boolean,
  category:Object,
  showDropdown:Boolean
});
</script>
