<template>
<div class="flex gap-[10px]">
    <span @click="editDocument" class="material-symbols-outlined text-[15px] text-[#b1bbd0]">edit</span>
    <span @click="handleDelete" class="material-symbols-outlined text-[18px] text-[#ff238d]">delete</span>
    <span class="material-symbols-outlined text-[18px] text-[#b1bbd0]" :class="{'text-[#ff238d]':dragIcon}">height</span>
</div>
</template>

<script setup>


const props=defineProps({
    categoryId:Number,
    documentId:Number,
    documentCategories:Array,
    dragIcon:Boolean,    
})

const emit =defineEmits(['deleteCategory', 'deleteDocument'])

const editDocument = (event) =>{
    event.stopPropagation()
    props.documentCategories.map((elm)=>elm.id===props.categoryId?elm.input=true:elm)
}

const handleDelete = (event)=>{
    event.stopPropagation()
    emit('deleteCategory',props.categoryId)
}

</script>