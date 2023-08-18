<template>


   <div>

      <div v-for="data in todos.data"  class="card my-5 mx-5">

    <div class="card-content">
      <div class="media">
        <div class="media-left">

        </div>
        <div class="media-content">
          <p class="title is-4">{{ data.name }}</p>
          <p class="subtitle is-6">{{ data.importance }}</p>

        </div>
      </div>

      <div class="content">
        {{ data.description }}<a>Done: {{ data.completed }}</a>.

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

      const todo = reactive({
        name: '',
        description: '',
        importance: '',
        category:'',
        image: '',
        lastday:'',
        completed:'',

      })
      const todos = ref([])

      async function fetchTodos() {
  try {
    const response = await axios.get('http://localhost:5000/api/todo?page=1');

    //todos.value = []; // Mevcut todo listesini temizleyelim

    todos.value = response.data;
    console.log(todos.data);

    //console.log(todos.value.data[0]);
  } catch (error) {
    console.error(error);
  }
}
     // Komponent yüklendiğinde otomatik olarak verileri çekmek için onMounted kullanılır
     onMounted(() => {
      fetchTodos();
    });

      return {
        todo,
        todos,
        fetchTodos,



      }
    }

  }
  </script>

  <style lang="scss">
  .done {
    text-decoration: line-through;
  }

  </style>
