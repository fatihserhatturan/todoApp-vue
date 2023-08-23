<template>
  <div class="field">
    <form @submit.prevent="addTodo">
      <div class="field">
        <label class="label">
          <h1>{{ Select === 'en' ? 'Name' : 'İsim' }}</h1>
        </label>
        <div class="control">
          <input v-model="todo.name" class="input" type="text" placeholder="Text input">
        </div>
      </div>

      <div class="field">
        <label class="label">
          <h1>{{ Select === 'en' ? 'Last Day' : 'Son Gün' }}</h1>
        </label>
        <div class="control">
          <datepicker v-model="todo.lastday" />
        </div>
      </div>

      <div class="field">
        <label class="label">
          <h1>{{ Select === 'en' ? 'Image' : 'Resim' }}</h1>
        </label>
        <div class="control">
          <input v-model="todo.image" class="input" type="text" placeholder="Image Url İnput">
        </div>
        <input type="file" ref="fileInput" style="display: none" @change="uploadImage">
        <button @click="openFileInput">{{ Select === 'en' ? 'Choose Image' : 'Resim Seç' }}</button>
      </div>

      <div class="field">
        <label class="label">
          <h1>{{ Select === 'en' ? 'Category' : 'Kategori' }}</h1>
        </label>
        <div class="control">
          <div class="select">
            <select v-model="todo.category">
              <option value="flight">{{ Select === 'en' ? 'Flight' : 'Uçuş' }}</option>
              <option value="reservation">{{ Select === 'en' ? 'Reservation' : 'Rezervasyon' }}</option>
              <option value="meeting">{{ Select === 'en' ? 'Meeting' : 'Buluşma' }}</option>
              <option value="job">{{ Select === 'en' ? 'Job' : 'İş' }}</option>
              <option value="dinner">{{ Select === 'en' ? 'Dinner' : 'Yemek' }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="field">
        <label class="label">
          <h1>{{ Select === 'en' ? 'Description' : 'Açıklama' }}</h1>
        </label>
        <div class="control">
          <textarea v-model="todo.description" class="textarea" placeholder="Textarea"></textarea>
        </div>
      </div>

      <div class="field">
        <label class="label">
          <h1>{{ Select === 'en' ? 'Importance' : 'Önem Derecesi' }}</h1>
        </label>
        <div class="control">
          <label class="radio">
            <input v-model="todo.importance" type="radio" value="High" name="question">
            {{ Select === 'en' ? 'High' : 'Yüksek' }}
          </label>
          <label class="radio">
            <input v-model="todo.importance" type="radio" value="Mid" name="question">
            {{ Select === 'en' ? 'Mid' : 'Orta' }}
          </label>
          <label class="radio">
            <input v-model="todo.importance" type="radio" value="Low" name="question">
            {{ Select === 'en' ? 'Low' : 'Düşük' }}
          </label>
        </div>
      </div>

      <div class="field is-grouped">
        <div class="control">
          <button class="button is-link">
            {{ Select === 'en' ? 'Submit' : 'Onayla' }}
          </button>
        </div>
        <div class="control">
          <button type="submit" class="button is-link is-light">
            {{ Select === 'en' ? 'Cancel' : 'İptal' }}
          </button>
        </div>
      </div>
    </form>

    <div v-for="todo in todos" class="card my-5 mx-5">
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ todo.name }}</p>
            <p :class="{ done: todo.completed }" @click="done(todo)" class="subtitle is-6">{{ todo.importance }}</p>
            <p :class="{ done: todo.completed }" @click="done(todo)" class="subtitle is-6">{{ todo.category }}</p>
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
  methods: {
        openFileInput() {
            this.$refs.fileInput.click(); // Dosya seçimini başlatmak için gizli input alanını tıklama
        },
        uploadImage(event) {
            const file = event.target.files[0]; // Seçilen dosyayı al
            const filePath = URL.createObjectURL(file); // Dosya yolunu al

            // Resim yolu verisine atanır
            this.todo.image = filePath;
        }
    },


    setup(){
      //const todo = ref('')
      const todo = reactive({
        name: '',
        image:'',
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
        format,
        selectedFile: null,
      }

    },



  }
  </script>

<style lang="scss">
.card {
  padding: 20px;
  background-color: #f5f5f5;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.field {
  margin-bottom: 20px;
}

.label h1 {
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
  color: #333;
}

.control input,
.control select,
.control textarea {
  width: 100%;
  border: 1px solid #ccc;
  padding: 8px;
  border-radius: 5px;
  font-size: 14px;
}

.select select {
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  padding: 8px;
  border-radius: 5px;
  font-size: 14px;
  width: 100%;
  background-color: white;
}

.button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 10px;
}

.button.is-light {
  background-color: transparent;
  border: 1px solid #007bff;
  color: #007bff;
}

.button.is-light:hover {
  background-color: #007bff;
  color: white;
}

.done {
  text-decoration: line-through;
}

.my-5 {
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
}

.mx-5 {
  margin-left: 1.5rem;
  margin-right: 1.5rem;
}
</style>
