<script setup>
import Card from "./components/Card.vue";
import {ref} from "vue";

const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMessage = ref("")

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = ()=>{
  if (newNote.value.length < 10){
    return errorMessage.value = "Note harus lebih dari 10 karakter"
  }
  notes.value.push({
    id : Math.floor(Math.random() * 1000000),
    text : newNote.value,
    date : new Date(),
    bgColor : getRandomColor(),
  })
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
}

</script>
<template>

  <div class="container mx-auto max-w-screen-md px-4">
    <header class="flex flex-row justify-between my-5">
      <h1 class="text-2xl font-sans font-semibold text-blue-900">Notes</h1>
      <button @click="showModal = true" class="text-base bg-blue-600 hover:bg-blue-800 ring-1 hover:ring-blue-400 focus:ring-blue-400 py-2 px-3.5 text-white rounded-full">+</button>
    </header>
    <!--modal-->
    <div v-if="showModal" class="fixed w-full h-full top-0 left-0 z-50 bg-slate-900/75 p-4 flex justify-center items-center">
      <div class="bg-white w-full h-auto max-w-2xl p-4 rounded-lg">
          <textarea v-model="newNote" rows="8" class="w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-blue-500"></textarea>
          <p class="text-red-700" v-if="errorMessage">{{ errorMessage }}</p>
        <!--button-->
          <div class="flex items-center space-x-2 mt-4">
            <button @click="addNote" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">Add Note</button>
            <button @click="showModal = false" class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-blue-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10  dark:focus:ring-gray-600">Close</button>
          </div>
      </div>
    </div>
    <hr class="mb-5">
    <div class="grid sm:grid-cols-3 md:grid-cols-4 gap-4">
      <Card v-for="note in notes" :key="note.id" :style="{backgroundColor: note.bgColor }">
        <div class="min-h-[100px]">
          <p>{{ note.text }}</p>
        </div>
        <p class="text-sm text-slate-700">{{ note.date.toLocaleDateString("en-US") }}</p>
      </Card>
    </div>
  </div>
</template>
