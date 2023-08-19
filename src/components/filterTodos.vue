<template>

    <div>
        <div>
            <div class="field">

  <div class="control is-flex">
    <div class="field-item">
      <label class="label-item">Category</label>

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

    <div class="field-item">
      <label class="label-item">Importance</label>
      <div class="select">
        <select v-model="importanceSelected">
          <option value="High">High</option>
          <option value="Mid">Mid</option>
          <option value="Low">Low</option>
        </select>
      </div>
    </div>

    <div class="field-item">
      <label class="label-item">Complete</label>
      <div class="select">
        <select v-model="completeSelected">
          <option value="false">Uncomplete</option>
          <option value="true">Complete</option>
        </select>
      </div>
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
       const categorySelected=ref();
       const importanceSelected =ref();
       const completeSelected = ref();
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

      async function categoryFilter(){
        console.log(categorySelected.value)
        const url = 'http://localhost:5000/api/filter/category';
        const response = await axios.post(url,{
            category:categorySelected.value,

        });

        todos.value = response.data;
        console.log(todos.value[0])
      }

      async function importanceFilter(){
        console.log(importanceSelected.value)
        const url = 'http://localhost:5000/api/filter/importance';
        const response = await axios.post(url,{
            importance:importanceSelected.value,

        });

        todos.value = response.data;
      }

      async function completeFilter(){
        console.log(completeSelected.value)
        const url = 'http://localhost:5000/api/filter/complete';
        const response = await axios.post(url,{
            completed:completeSelected.value,

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

    watch(importanceSelected, () => {
      importanceFilter();

    });

    watch(completeSelected, () => {
      completeFilter();

    });


       return {
         todos,
         fetchTodos,
         categorySelected,
         importanceSelected,
         completeSelected,
         categoryFilter,
         importanceFilter,
         completeFilter,

       }
     }

   }
   </script>

   <style lang="scss">

   .field-item {
  margin-right: 20px;
}

.label-item {
  margin-bottom: 5px;
}

   </style>
