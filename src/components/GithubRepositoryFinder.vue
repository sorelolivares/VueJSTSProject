<script lang="ts" setup>
import axios from 'axios'
import { ref} from 'vue';
const languages = ref<{title:string, value: string}[]>([])

const getRandomInt = (min : number, max :number) =>  {
  const minCeiled = Math.ceil(min);
  const maxFloored = Math.floor(max);
  return Math.floor(Math.random() * (maxFloored - minCeiled) + minCeiled); // The maximum is exclusive and the minimum is inclusive
}
const getLanguages = async () => {
  const data = await axios.get('https://raw.githubusercontent.com/kamranahmedse/githunt/master/src/components/filters/language-filter/languages.json')
  languages.value = data.data
}
const programmingLanguage = ref('')
const token = ref("")
const repositorie = ref({full_name:'',description:'', html_url: '', stargazers_count: '', language: '', open_issues:'', forks: ''})
const loading = ref(0)
const getRepositories = async () => {
  loading.value = 1
  const headers = {
    "Accept": "application/vnd.github+json",
    "Content-Type": "application/json",
    'X-GitHub-Api-Version': '2022-11-28',
    'Authorization': `Token ${token.value}`
  }
  const data = await axios.get('https://api.github.com/search/repositories',{
    params:{
      q : programmingLanguage.value,
      language : programmingLanguage.value
    },headers
  })
  const items = data.data.items
  const getRandomRepositorie =  items[getRandomInt(0, items.length)]
  if(getRandomRepositorie.language !== programmingLanguage.value ){
    getRepositories()
  }else{
    repositorie.value = getRandomRepositorie
    loading.value = 0
  }
}

getLanguages()
</script>
<template>
  <div class="w-full flex justify-center items-center">
    <div class="w-1/5">
      <div class="w-full mb-4">
        <select name="" @change="getRepositories" v-model="programmingLanguage"  id="" class="w-full py-2 bg-gray-100 px-3 border border-gray-300 rounded-xl">
          <option value="" selected>....</option>
          <option :value="language.value"  v-for="language,index of languages" :key="index" class="w-full py-2 px-3 border border-gray-300 rounded-xl">{{ language.title }}</option>
        </select>
      </div>
      <div class="w-full h-48 flex justify-center items-center mb-4">
        <div class="w-full h-full bg-gray-100 flex flex-col justify-around items-center rounded-xl text-gray-700 text-sm border border-gray-300">
          <div v-if="programmingLanguage" class="w-full h-full flex items-center justify-center">
          <div class="w-full h-full select-none cursor-wait flex justify-center items-center" v-if="loading > 0">
            Cargando...
          </div>
          <div v-else class="w-full h-full flex flex-col justify-around items-start px-5 py-2 ">
            <div class="text-base">
              {{ repositorie.full_name }}
            </div>
            <div class="">
              {{ repositorie.description }}
            </div>
            <div class="">
              <a :href="repositorie.html_url" target="_blank">{{ repositorie.html_url }}</a>
            </div>
            <div class="text-xs w-full flex flex-row justify-around items-center">
              <div class="flex flex-row justify-center items-center">
                <svg xmlns="http://www.w3.org/2000/svg" fill="yellow" viewBox="0 0 24 24" stroke-width="1.5" stroke="yellow" class="size-5">
  <path stroke-linecap="round" stroke-linejoin="round" d="M5.25 7.5A2.25 2.25 0 0 1 7.5 5.25h9a2.25 2.25 0 0 1 2.25 2.25v9a2.25 2.25 0 0 1-2.25 2.25h-9a2.25 2.25 0 0 1-2.25-2.25v-9Z" />
</svg>


                {{ repositorie.language }}
              </div>
              <div class="flex flex-row justify-center items-center">
                <svg xmlns="http://www.w3.org/2000/svg" fill="gray" viewBox="0 0 24 24" stroke-width="1.5" stroke="gray" class="size-5">
  <path stroke-linecap="round" stroke-linejoin="round" d="M11.48 3.499a.562.562 0 0 1 1.04 0l2.125 5.111a.563.563 0 0 0 .475.345l5.518.442c.499.04.701.663.321.988l-4.204 3.602a.563.563 0 0 0-.182.557l1.285 5.385a.562.562 0 0 1-.84.61l-4.725-2.885a.562.562 0 0 0-.586 0L6.982 20.54a.562.562 0 0 1-.84-.61l1.285-5.386a.562.562 0 0 0-.182-.557l-4.204-3.602a.562.562 0 0 1 .321-.988l5.518-.442a.563.563 0 0 0 .475-.345L11.48 3.5Z" />
</svg>

                {{ repositorie.stargazers_count }}
              </div>
              <div class="flex flex-row justify-center items-center">
                <svg xmlns="http://www.w3.org/2000/svg" fill="gray" viewBox="0 0 24 24" stroke-width="1.5" stroke="gray" class="size-5">
  <path stroke-linecap="round" stroke-linejoin="round" d="M7 5C7 3.89543 7.89543 3 9 3C10.1046 3 11 3.89543 11 5C11 5.74028 10.5978 6.38663 10 6.73244V14.0396H11.7915C12.8961 14.0396 13.7915 13.1441 13.7915 12.0396V10.7838C13.1823 10.4411 12.7708 9.78837 12.7708 9.03955C12.7708 7.93498 13.6662 7.03955 14.7708 7.03955C15.8753 7.03955 16.7708 7.93498 16.7708 9.03955C16.7708 9.77123 16.3778 10.4111 15.7915 10.7598V12.0396C15.7915 14.2487 14.0006 16.0396 11.7915 16.0396H10V17.2676C10.5978 17.6134 11 18.2597 11 19C11 20.1046 10.1046 21 9 21C7.89543 21 7 20.1046 7 19C7 18.2597 7.4022 17.6134 8 17.2676V6.73244C7.4022 6.38663 7 5.74028 7 5Z" />
</svg>

                {{ repositorie.forks }}
              </div>
              <div class="flex flex-row justify-center items-center">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-5">
  <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m9-.75a9 9 0 1 1-18 0 9 9 0 0 1 18 0Zm-9 3.75h.008v.008H12v-.008Z" />
</svg>

                {{ repositorie.open_issues }}
              </div>
            </div>
          </div>
        </div>
        <div v-else class="">
          Please Select An Option
        </div>
        </div>
      </div>
      <div v-if="programmingLanguage" class="w-full">
        <button @click="getRepositories" :disabled="loading>0"  :class="[loading > 0 ? 'py-2 w-full bg-gray-900 bg-opacity-50 cursor-wait text-gray-100 rounded-xl' : 'py-2 w-full bg-gray-900 text-gray-100 rounded-xl']">Refresh</button>
      </div>
    </div>
  </div>
</template>
