<script setup>
import {ref, computed} from 'vue'

const title = ref('Shopping List App');
const chracterCount = computed(() => {
  return newItem.value.length
})



const items = ref([])

const newItem = ref("");
const newItemPriority = ref(false);
const editing = ref(false);

const saveItem = () => {
  // when we are referencing reactive ref inside script tag we have to use .value
  items.value.unshift({id: items.value.length +1, label: newItem.value ,     purchased: false,
    important: newItemPriority.value,})
  newItem.value = ""
  newItemPriority.value = false
}

const doEdit = (e) => {
  editing.value = e
  newItem.value = ""
}

const handlePurchase = (item) => {
  item.purchased = !item.purchased
}

const handleImportant = (item) => {
  item.important = !item.important
}
</script>

<template>
<section class="bg-gray-100 w-full h-screen p-0 m-0 overflow-hidden flex items-center justify-center">

<main class="w-2/3 bg-white p-5 rounded-md shadow-sm">
<div><header class="flex justify-between items-center">
  <!-- page title -->
  <h1 class="text-2xl">{{title}}</h1>
  <!-- cancel editing btn -->
  <button class="bg-red-400 py-1 px-2 text-white rounded-md hover:bg-red-500 transition-all" v-if="editing" @click="doEdit(false)">Cancel</button>
  <!-- add item editing btn -->
  <button v-else @click="doEdit(true)" class="bg-blue-400 py-1 px-2 text-white rounded-md hover:bg-blue-500 transition-all">Add item</button>
</header>
  
  <!-- add new item form -->
<form @submit.prevent="saveItem" v-if="editing" class="my-5">
 <div class="flex items-center"> <!-- input for item label -->
  <input 
  class="border border-black rounded-md py-1 px-2 mr-5"
type="text"
 v-model="newItem"
  placeholder="Add an item"
   />
   <p>{{chracterCount}}/200</p> </div>
   <!-- handle priority and save -->
 <div class="flex  items-center mt-2"> <label class="mr-2"><input class="mr-1" type="checkbox" v-model="newItemPriority" />High priority</label>
<button class="border border-green-400 py-1.5 px-2 rounded-lg text-sm 
 uppercase hover:bg-green-400 transition-all cursor-pointer hover:text-white" :disabled="!newItem.length" type="submit">Save item</button></div>
 
</form>


 <!-- shopping list -->
<ul class="flex flex-col py-2">

  <li v-for="({id, label, purchased, important}, index) in items" :key="id" :class="{'line-through text-gray-300': purchased}" class="mb-1 text-gray-500 cursor-pointer" title="click to make important" @click="handleImportant(items[index])">
  <!-- checkbox for purchase -->
  <input type="checkbox" class="mr-1 cursor-pointer" @change="handlePurchase(items[index])">
  <span  :class="{'text-red-500': important&& !purchased}">{{label}}</span>
  </li>
</ul>
<!-- shopping list --></div></main>
  
  </section>

</template>
