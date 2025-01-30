<script lang="ts" setup>
import { ref} from 'vue';
const tasks = ref<{id: string, name:string, status: boolean}[]>([])
const newTask = ref({
  id:crypto.randomUUID(),
  name:'',
  status:false
})
const selectedTasks = ref<{id: string, name:string, status: boolean}[]>([])
const addTask = () =>{
  tasks.value.push(newTask.value)
  newTask.value = {
    id:crypto.randomUUID(),
    name:'',
    status:false
  }

}
const removeTask = (index:number) => {
  tasks.value.splice(index,1)
}

const validateTaskExistence = (id:string) => {
  let val = false
  selectedTasks.value.map((el) => {
    if (el.id === id) return true
    val = true
  })
  return val
}

const handleCheckboxChange = (item: {id: string, name:string, status: boolean}) =>{
  //const key = `task${id}`
  const index = selectedTasks.value.indexOf(item)
  if(item.status === true){
    selectedTasks.value.splice(index,1)
  }else{
    selectedTasks.value.push(item)
  }
}

</script>
<template>
  <div class="w-full flex flex-row justify-around">
    <div class="flex flex-col">
      <input v-model="newTask.name" type="text" class="px-3 py-1 border border-gray-900 rounded">
      <button @click="addTask" class="px-3 py-1 bg-blue-600 text-white rounded">Agregar Tarea</button>
    </div>
    <div class="">
      <p>Tareas</p>
      <ul v-for="item,index of tasks" :key="'task'+index" class="">
        <li class="flex flex-row items-center justify-center"><input v-model="item.status" type="checkbox" class="mr-2 w-4 h-4 bg-yellow-600" @click="handleCheckboxChange(item)" /><p :class="[ validateTaskExistence(`task${index}`) ? 'line-through' : '']">{{ item.name }}</p><button @click="removeTask(index)" class="px-3 py-1 bg-red-700 text-white ml-2 rounded">Borrar</button></li>
      </ul>
    </div>
  </div>
</template>
