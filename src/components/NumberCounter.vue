<script setup lang="ts">
import {ref, computed} from 'vue'
const count = ref(0)
const favoriteNumbers = ref<number[]>([])
//const favoriteNumbers: number[] = ref([])
const increment = () => {
  count.value++
}
const decrement = () => {
  count.value--
}
const addFavoriteNumber = () =>{
  favoriteNumbers.value.push(count.value)
  console.log(favoriteNumbers)
}

const reset = () =>{
  count.value = 0
  favoriteNumbers.value = []
}
// const countStyle = () =>{
//   return count.value < 0 ? 'color:red' : 'color:green'
// }

const classCounter = computed( () => {
  let color = ''
  if(count.value == 0){
    color = 'text-gray-800'
  }
  if(count.value > 0){
    color = 'text-green-700'
  }
  if(count.value < 0){
    color = 'text-red-700'
  }
  favoriteNumbers.value.forEach(el => {
    if (el == count.value) {
      color = 'text-gray-300'
    }
  })
  return color
})

const validateExistence = computed( () => {
  let val = false
  favoriteNumbers.value.forEach(element => {
    if(element === count.value){
      val = true
    }
  });
  return val
})

</script>
<template>
  <div class=" w-1/2 flex flex-row justify-around text-xl mx-auto border">
    <div class="flex flex-col">
      <div class="mx-auto text-5xl mb-4">
        <span :class="classCounter">{{ count }}</span>
      </div>
      <div class="mx-auto">
        <button class="bg-green-400 p-2 rounded-lg mx-2" @click="increment()">Aumentar</button>
        <button class="bg-yellow-400 p-2 rounded-lg mx-2" @click="decrement()">Disminuir</button>
        <button :class="[ !validateExistence ? 'bg-blue-400 p-2 rounded-lg mx-2'  : 'bg-blue-400 p-2 rounded-lg cursor-not-allowed mx-2 opacity-50']" :disabled="validateExistence" @click="addFavoriteNumber()">Add number</button>
        <button class="bg-gray-400 p-2 rounded-lg mx-2" @click="reset()">Resetear</button>
      </div>
    </div>
    <div class="text-2xl">
      <h1>Favorite Numbers List</h1>
      <ul class="flex justify-center" v-for="number,index in favoriteNumbers" :key="index">
        <li>{{ number }}</li>
      </ul>
    </div>
  </div>
</template>
