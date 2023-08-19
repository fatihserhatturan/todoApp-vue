<template>


   <div>
    <div class="pagination-buttons">
  <button class="button is-success is-light" @click="changePage('prev')">
    &larr;
  </button>
  <button class="button is-success is-light" @click="changePage('next')">
     &rarr;
  </button>
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
        <button @click="completeTodo(data._id)" class="button is-success is-focused" :disabled="data.completed">Complete</button>
        <button @click="deleteTodo(data._id)" class="button is-danger is-focused">Delete</button>
        <button @click="isOpen=true" class="button is-link is-focused">Update</button>





        <br>

      </div>
    </div>
  </div>



    </div>



  </template>

  <script>
  import { ref, reactive, onMounted } from 'vue';
  import axios from 'axios';


  export default{
    components: {
  },

    setup(){

        const isOpen=ref(false);
        console.log(isOpen.value);

      const currentPage = ref(1);

      const todos = ref([])





      async function fetchTodos() {
  try {
    const url = 'http://localhost:5000/api/todo?page=' + currentPage.value;
    const response = await axios.get(url);


    todos.value = response.data;

  } catch (error) {
    console.error(error);
  }
}
async function completeTodo(id){
    try {
        const url ='http://localhost:5000/api/todo/'+id;
        const response = await axios.put(url,{
            completed:true,
        });

    } catch (error) {
        console.error(error)

    }
}
async function deleteTodo(id){
    try {
        const url ='http://localhost:5000/api/todo/'+id;
        const response =await axios.delete(url);

    } catch (error) {
        console.error(error);

    }
}
function changePage(action) {

      if (action === 'prev') {
        if (currentPage.value > 1) {
          currentPage.value--;
          fetchTodos();

        }
      } else if (action === 'next') {
        currentPage.value++;

        fetchTodos();
      }

    }
     // Komponent yüklendiğinde otomatik olarak verileri çekmek için onMounted kullanılır
     onMounted(() => {
      fetchTodos();
    });
      return {
        todos,
        fetchTodos,
        changePage,
        completeTodo,
        deleteTodo,


      }
    }

  }
  </script>

  <style lang="scss">
.pagination-buttons {
  display: flex;
  justify-content: space-between; /* Aralarında boşluk bırakarak yatayda hizalar */
  margin-top: 10px; /* İhtiyaca göre boşluk bırakabilirsiniz */
}

.pagination-buttons button {
  display: flex;
  align-items: center;
}

  </style>
