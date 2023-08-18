<template>

    <div>
        <div>
      <!-- Pagination Buttons -->
      <div class="field">
    <label class="label">Category</label>
    <div class="control">
      <div class="select">
        <select v-model="categorySelected">

          <option value="flight">Flight</option>
          <option value="reservation">Reservation</option>
          <option value="meeting">Meeting</option>
          <option value="job">Job</option>
          <option value="dinner">Dinner</option>

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
   import { ref, reactive, onMounted,watch } from 'vue';
   import axios from 'axios';

   export default{
     components: {
   },

     setup(){


       const todos = ref([])
       const categorySelected=ref();
      // console.log(category);

       async function fetchTodos() {
   try {
     const url = 'http://localhost:5000/api/todo?page=3';
     const response = await axios.get(url);



     todos.value = response.data;

   } catch (error) {
     console.error(error);
   }
 }

      async function categoryFilter(){
        console.log(categorySelected.value)
        const url = 'http://localhost:5000/api/filter/category';
        const response = await axios.post(url,{
            category:categorySelected.value,

        });
        


        todos.value = response.data;
      }

      // Komponent yüklendiğinde otomatik olarak verileri çekmek için onMounted kullanılır
      onMounted(() => {
       fetchTodos();
     });

     watch(categorySelected, () => {
      categoryFilter();
    });
       return {
         todos,
         fetchTodos,
         categorySelected,
         categoryFilter,

       }
     }

   }
   </script>

   <style lang="scss">

   </style>
