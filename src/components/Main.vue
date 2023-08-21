<template>
    <div  class="card ">
      <form @submit.prevent="addTodo">
        <div class="field">
    <label class="label">

      <div v-if="Select === 'en'">
      <h1>Name</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>İsim</h1>

    </div>
    </label>
    <div class="control">
      <input v-model="todo.name" class="input" type="text" placeholder="Text input">
    </div>
  </div>
  <div class="field">

        <label class="label">  <div v-if="Select === 'en'">
      <h1>Last Day</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Son Gün</h1>

    </div></label>
        <div class="control">
          <datepicker v-model="todo.lastday" />
        </div>
      </div>



  <div class="field">
    <label class="label">  <div v-if="Select === 'en'">
      <h1>Image</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Resim</h1>

    </div></label>
    <div class="control ">
      <input v-model="todo.image"  class="input" type="text" placeholder="Image Url İnput">


    </div>

  </div>

  <div class="field">
    <label class="label">  <div v-if="Select === 'en'">
      <h1>Category</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Kategori</h1>

    </div></label>
    <div class="control">
      <div class="select">
        <select v-model="todo.category">

          <option value="flight">  <div v-if="Select === 'en'">
      <h1>Flight</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Ucuş</h1>

    </div></option>
          <option value="reservation">  <div v-if="Select === 'en'">
      <h1>Reservation</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Rezervasyon</h1>

    </div></option>
          <option value="meeting">  <div v-if="Select === 'en'">
      <h1>Meeting</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Buluşma</h1>

    </div></option>
          <option value="job">  <div v-if="Select === 'en'">
      <h1>Job</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>İş</h1>

    </div></option>
          <option value="dinner">  <div v-if="Select === 'en'">
      <h1>Dinner</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Yemek</h1>

    </div></option>

        </select>
      </div>
    </div>
  </div>

  <div class="field">
    <label class="label">  <div v-if="Select === 'en'">
      <h1>Description</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Açıklama</h1>

    </div></label>
    <div class="control">

      <textarea v-model="todo.description" class="textarea" placeholder="Textarea"></textarea>
    </div>
  </div>



  <div class="field">
    <div class="control">
      <label class="radio">
        <input v-model="todo.importance" type="radio" value="High" name="question">
        <div v-if="Select === 'en'">
      <h1>High</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Yüksek</h1>

    </div>

      </label>
      <label class="radio">
        <input v-model="todo.importance" type="radio" value="Mid" name="question">
        <div v-if="Select === 'en'">
      <h1>Mid</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Orta</h1>

    </div>
      </label>
      <label class="radio">
        <input v-model="todo.importance" type="radio" value="Low" name="question">
        <div v-if="Select === 'en'">
      <h1>Low</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Düşük</h1>

    </div>
      </label>
    </div>
  </div>





  <div class="field is-grouped">
    <div class="control">
      <button class="button is-link">  <div v-if="Select === 'en'">
      <h1>Submit</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>Onayla</h1>

    </div></button>
    </div>
    <div class="control">
      <button type="submit" class="button is-link is-light">
          <div v-if="Select === 'en'">
      <h1>Cancel</h1>
    </div>

    <div v-else-if="Select === 'tr'">
      <h1>İptal</h1>

    </div></button>
    </div>
  </div>

      </form>
      <div v-for="todo in todos" class="card my-5 mx-5">

    <div class="card-content">
      <div class="media">
        <div class="media-left">

        </div>
        <div class="media-content">
          <p class="title is-4">{{ todo.name }}</p>
          <p :class="{done:todo.completed}" @click="done(todo)" class="subtitle is-6">{{ todo.importance }}</p>
          <p :class="{done:todo.completed}" @click="done(todo)" class="subtitle is-6">{{ todo.category }}</p>
        </div>
      </div>

      <div class="content">
        {{ todo.description }}<a>Done: {{ todo.done }}</a>.

        <br>
        <time datetime="2016-1-1">{{ format(new Date(todo.lastday), 'MM/dd/yyyy') }}</time>
      </div>
    </div>
  </div>


    </div>

  </template>

  <script>
  import{ref,reactive} from 'vue'
  import Datepicker from 'vue3-datepicker';
  import { format } from 'date-fns';
  import axios from 'axios';


  export default{
    name: 'Main',
    props:[
      'Select'

    ],
    components: {
    Datepicker,
  },
    setup(){
      //const todo = ref('')
      const todo = reactive({
        name: '',
        image: '',
        description: '',
        importance: '',
        category:'',
        lastday:''

      })
      const todos = ref([])
      //console.log(selectedLanguage);

     async function addTodo(){

      try {
        const response = await axios.post('http://localhost:5000/api/todo', {
          completed: false,
          description: todo.description,
          name: todo.name,
          image: todo.image,
          importance: todo.importance,
          category: todo.category,
          lastday: todo.lastday,

        });
        console.log("girdi");

        todos.value.push(response.data); // API'den dönen veriyi ekler
      } catch (error) {
        console.error(error);
      }


      }
      function done(todo){
        todo.done = !todo.done
      }

      return {
        todo,
        todos,
        addTodo,
        done,
        format
      }
    }

  }
  </script>

  <style lang="scss">
  .done {
    text-decoration: line-through;
  }

  </style>
