<template>


   <div>
    <div>
      <!-- Pagination Buttons -->
      <button @click="changePage('prev')">Previous</button>
      <button @click="changePage('next')">Next</button>
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

      const currentPage = ref(1);
     // const id=ref();
      const todos = ref([])
      async function fetchTodos() {
  try {
    const url = 'http://localhost:5000/api/todo?page=' + currentPage.value;
    const response = await axios.get(url);
    //todos.value = []; // Mevcut todo listesini temizleyelim

    todos.value = response.data;
    //console.log(todos.data);

    //console.log(todos.value.data[0]);
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
function changePage(action) {
    //console.log(currentPage.value);
    //console.log(action);
      if (action === 'prev') {
        if (currentPage.value > 1) {
          currentPage.value--;
          fetchTodos();
          //console.log(currentPage.value);
        }
      } else if (action === 'next') {
        currentPage.value++;
       // console.log(currentPage.value);
        fetchTodos();
      }
     // console.log(currentPage.value);
    }
     // Komponent yüklendiğinde otomatik olarak verileri çekmek için onMounted kullanılır
     onMounted(() => {
      fetchTodos();
    });
      return {
        todos,
        fetchTodos,
        changePage,
        completeTodo

      }
    }

  }
  </script>

  <style lang="scss">
  .done {
    text-decoration: line-through;
  }

  </style>
