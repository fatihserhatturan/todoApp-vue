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
          <p class="subtitle is-6">{{ formatDate(data.lastday) }}</p>
        <br>

        </div>
      </div>

      <div class="content">
        <br>
        {{ data.description }}
        <a>Done: {{ data.completed }}</a>.
        <button class="custom-button"  @click="completeTodo(data._id)" :disabled="data.completed">  <div v-if="Select === 'en'">
      <h1>Complete</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Tamamlandı</h1>

    </div></button>
        <button class="custom-button"  @click="deleteTodo(data._id)">  <div v-if="Select === 'en'">
      <h1>Delete</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Sil</h1>

    </div></button>
        <button class="custom-button"   @click="openUpdateModal(data._id)">  <div v-if="Select === 'en'">

      <h1>Update</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Güncelle</h1>

    </div></button>

        <br>

      </div>
    </div>
  </div>



    </div>

    <update-todo-modal
      :is-open="isOpen"
      :selected-Todo-Object="selectedTodoObject"
      :update-todo="updateTodo"
      :close-modal="closeModal"
    />

  </template>

  <script>
  import { ref, reactive, onMounted } from 'vue';
  import axios from 'axios';
  import UpdateTodoModal from './UpdateTodoModal.vue'// UpdateTodoModal bileşeni dosya yoluna göre ayarlayın
  import { id } from 'date-fns/locale';
  import { format } from "date-fns";


  export default{
    props:[
      'Select'

    ],
    components: {
        UpdateTodoModal,
  },
  methods: {
    formatDate(date) {
      return format(new Date(date), "MM/dd/yyyy");
    },
    // Diğer metotlar
  },

    setup(){

        const isOpen=ref(false);


      const currentPage = ref(1);

      const todos = ref([])
      const selectedTodo = ref(null);
      const selectedTodoObject = ref([]);




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

   async function openUpdateModal(id) {
        isOpen.value = true;
        selectedTodo.value = id;
        console.log(id);


        try {
        const url ='http://localhost:5000/api/todo/'+id;
        const response = await axios.get(url)

        selectedTodoObject.value = response.data;


        console.log(selectedTodoObject.value.name);



    } catch (error) {
        console.error(error)

    }

    }
   async function updateTodo() {

      // Güncelleme işlevselliğini burada gerçekleştirin
      if (selectedTodo.value) {
        // Örneğin, seçili todo nesnesinin adını güncelleyebilirsiniz
        selectedTodo.value = id;

        // Güncelleme işlevselliğini sunucuya gönderme gibi adımları gerçekleştirin
        // ...
      }
      closeModal();
    }
    function closeModal(){
       // console.log("girdi");
        isOpen.value=false;
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
        isOpen,
        selectedTodo,
        openUpdateModal,
        updateTodo,
        closeModal,
        selectedTodoObject,





      }
    }


  }
  </script>

  <style lang="scss">

.card-content {
  padding: 15px;
  border-top: 1px solid #ccc;
}

.media {
  display: flex;
  align-items: center;
}

.media-content {
  flex: 1;
  margin-left: 15px;
}

.title.is-4 {
  font-size: 1.25rem;
  margin-bottom: 0.25rem;
}

.subtitle.is-6 {
  font-size: 0.875rem;
  color: #888;
}

.content {
  margin-top: 15px;
}
.pagination-buttons {
  display: flex;
  justify-content: space-between; /* Aralarında boşluk bırakarak yatayda hizalar */
  margin-top: 10px; /* İhtiyaca göre boşluk bırakabilirsiniz */
}

.pagination-buttons button {
  display: flex;
  align-items: center;
}

.custom-button {
    background-color: white; /* Green */
    border: 2px solid #4CAF50;
  color: black;
  padding: 7px 18px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 8px;
  transition-duration: 0.4s;

}
.custom-button:hover {
  background-color: #4CAF50; /* Green */
  color: white;
}

.custom-button + .custom-button {
  margin-left: 15px;
}
.custom-button h1 {
  font-size: 0.875rem;
  margin: 0;
}

.custom-button:disabled {
  background-color: #ccc;
  color: #888;
  cursor: not-allowed;
}

.custom-button:hover:not(:disabled) {
  color: #007bff;
}

.card-content .content {
  margin-top: 15px;
  line-height: 1.5;
}

.card-content .content a {
  display: block;
  margin-top: 5px;
  color: #888;
}

.card-content .content button {
  margin-top: 10px;
}

.card-content .content button:last-child {
  margin-right: 0;
}

.card-content .content button .h1 {
  font-size: 0.875rem;
  margin: 0;
}

.card-content .content button .h1::first-letter {
  text-transform: uppercase;
}

.subtitle.is-6 + .content {
  margin-top: 10px;
}

.subtitle.is-6 + .content p {
  margin: 0;
  font-size: 0.875rem;
  color: #888;
}

@media (max-width: 768px) {
  .media-content {
    margin-left: 10px;
  }

  .custom-button + .custom-button {
    margin-left: 10px;
  }}

  </style>
