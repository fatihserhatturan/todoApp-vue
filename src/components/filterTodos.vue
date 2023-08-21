<template>

    <div>
        <div>
            <div class="field">

  <div class="control is-flex">
    <div class="field-item">
      <label class="label-item"><div v-if="Select === 'en'">
      <h1>Category</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Kategori</h1>

    </div></label>

      <div class="select">
        <select v-model="categorySelected">
          <option value="flight"><div v-if="Select === 'en'">
      <h1>Flight</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Uçuş</h1>

    </div></option>
          <option value="reservation"><div v-if="Select === 'en'">
      <h1>Reservation</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Rezervasyon</h1>

    </div></option>
          <option value="meeting"><div v-if="Select === 'en'">
      <h1>Meeting</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Buluşma</h1>

    </div></option>
          <option value="job"><div v-if="Select === 'en'">
      <h1>Job</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>İş</h1>

    </div></option>
          <option value="dinner"><div v-if="Select === 'en'">
      <h1>Dinner</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Yemek</h1>

    </div></option>
        </select>
      </div>
    </div>

    <div class="field-item">
      <label class="label-item"><div v-if="Select === 'en'">
      <h1>importance</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Önem</h1>

    </div></label>
      <div class="select">
        <select v-model="importanceSelected">
          <option value="High"><div v-if="Select === 'en'">
      <h1>High</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Yüksek</h1>

    </div></option>
          <option value="Mid"><div v-if="Select === 'en'">
      <h1>Mid</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Orta</h1>

    </div></option>
          <option value="Low"><div v-if="Select === 'en'">
      <h1>Low</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Düşük</h1>

    </div></option>
        </select>
      </div>
    </div>

    <div class="field-item">
      <label class="label-item"><div v-if="Select === 'en'">
      <h1>Complete</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Tamamlanma</h1>

    </div></label>
      <div class="select">
        <select v-model="completeSelected">
          <option value="false"><div v-if="Select === 'en'">
      <h1>Uncomplete</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Tamamlanmamış</h1>

    </div></option>
          <option value="true"><div v-if="Select === 'en'">
      <h1>Complete</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Tamamlanmış</h1>

    </div></option>
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
    props:[
      'Select'

    ],
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
