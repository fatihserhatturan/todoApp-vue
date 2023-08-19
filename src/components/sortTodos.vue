<template>

    <div>
        <div>
            <div class="field">
  <label class="label">Sort(Early-Late)</label>
  <div class="control is-flex">

    <div class="select">
      <select v-model="sortSelected">
        <option value="false">Sort Early</option>
        <option value="true">Sort Late</option>

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
