<template>

    <div>
        <div>
            <div class="field">
  <label class="label"><div v-if="Select === 'en'">
      <h1>Sort(Early-Late)</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Sırala(Erken-Geç)</h1>

    </div></label>
  <div class="control is-flex">

    <div class="select">
      <select v-model="sortSelected">
        <option value="false"><div v-if="Select === 'en'">
      <h1>Sort Early</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Erkene Sırala</h1>

    </div></option>
        <option value="true"><div v-if="Select === 'en'">
      <h1>Sort Late</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Geçe Sırala</h1>

    </div></option>

      </select>
    </div>

  </div>
</div>

    </div>

       <div v-for="data in todos.data"  class="card my-5 mx-5">

     <div class="card-content">
       <div class="media">
         <div class="media-left">

         </div>
         <div class="media-content">
           <p class="title is-4">{{ data.name }}</p>
           <p class="subtitle is-6">{{ data.importance }}</p>
         <br>

         </div>
       </div>

       <div class="content">
         <br>
         {{ data.description }}
         <a>Done: {{ data.completed }}</a>.

         <br>

       </div>
     </div>
   </div>

     </div>

   </template>

   <script>
   import { ref, reactive, onMounted,watch, } from 'vue';
   import axios from 'axios';

   export default{
    props:[
      'Select'

    ],
     components: {
   },
     setup(){
       const todos = ref([])
       const sortSelected = ref();
      // console.log(category);

       async function fetchTodos() {
   try {
     const url = 'http://localhost:5000/api/todo?page=3';
     const response = await axios.get(url);



     todos.value = response.data;
     console.log(todos.value);


   } catch (error) {
     console.error(error);
   }
 }

      async function Sorting(){
        try {

            console.log(sortSelected.value)
        let url = '';
       if(sortSelected.value === 'true'){
        console.log("girdi");
        url='http://localhost:5000/api/sort/late'};
       if(sortSelected.value === 'false'){
        console.log("girdi");
        url='http://localhost:5000/api/sort/early'};
       console.log(url);

       const response = await axios.get(url);

        todos.value = response.data;
        console.log(todos.value)

        } catch (error) {
            console.error(error);
        }
      }

      // Komponent yüklendiğinde otomatik olarak verileri çekmek için onMounted kullanılır
      onMounted(() => {
       fetchTodos();
     });

    watch(sortSelected, () => {
      Sorting();

    });

       return {
         todos,
         fetchTodos,
         sortSelected,
         Sorting,

       }
     }

   }
   </script>

   <style lang="scss">

   </style>
