<template>
    <div  class="card ">
      <form @submit.prevent="addTodo">
        <div class="field">
    <label class="label"> Todo Name</label>
    <div class="control">
      <input v-model="todo.name" class="input" type="text" placeholder="Text input">
    </div>
  </div>
  <div class="field">

        <label class="label">Last Day</label>
        <div class="control">
          <datepicker v-model="todo.lastday" />
        </div>
      </div>



  <div class="field">
    <label class="label">İmage </label>
    <div class="control ">
      <input v-model="todo.image"  class="input" type="text" placeholder="Image Url İnput">


    </div>

  </div>

  <div class="field">
    <label class="label">Category</label>
    <div class="control">
      <div class="select">
        <select v-model="todo.category">

          <option value="flight">Flight</option>
          <option value="reservation">Reservation</option>
          <option value="meeting">Meeting</option>
          <option value="job">Job</option>
          <option value="dinner">Dinner</option>

        </select>
      </div>
    </div>
  </div>

  <div class="field">
    <label class="label">Description</label>
    <div class="control">

      <textarea v-model="todo.description" class="textarea" placeholder="Textarea"></textarea>
    </div>
  </div>



  <div class="field">
    <div class="control">
      <label class="radio">
        <input v-model="todo.importance" type="radio" value="High" name="question">
        High
      </label>
      <label class="radio">
        <input v-model="todo.importance" type="radio" value="Mid" name="question">
        Mid
      </label>
      <label class="radio">
        <input v-model="todo.importance" type="radio" value="Low" name="question">
        Low
      </label>
    </div>
  </div>





  <div class="field is-grouped">
    <div class="control">
      <button class="button is-link">Submit</button>
    </div>
    <div class="control">
      <button type="submit" class="button is-link is-light">Cancel</button>
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
