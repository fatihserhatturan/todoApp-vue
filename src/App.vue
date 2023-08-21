<template>
  <div id="app">

    <Preloader :isLoading="isLoading" />

    <button class="button is-link is-rounded" @click="changeLanguage('en')">En</button>
    <button class="button is-success is-rounded" @click="changeLanguage('tr')">Tr</button>


    <div class="centered-container">

      <div class="image-container">
        <img
          src=".\images\todo.gif"
          class="button-image"
        />
      </div>

      <div class="content-container">
      <div class="button-container">
        <button class="button is-success is-light" @click="loadMainComponent">

          <div v-if="selectedLanguage === 'en'">
      <h1>Add Todo</h1>
    </div>

    <div v-else-if="selectedLanguage === 'tr'">
      <h1>Todo Ekle</h1>

    </div>


  </button>
        <button class="button is-success is-light" @click="loadTodosComponent">

          <div v-if="selectedLanguage === 'en'">
      <h1>Get Todos</h1>
    </div>

    <div v-else-if="selectedLanguage === 'tr'">
      <h1>Todoları Getir</h1>

    </div>
        </button>
        <button class="button is-success is-light" @click="loadfilterTodosComponent">

          <div v-if="selectedLanguage === 'en'">
      <h1>Filter Todos</h1>
    </div>

    <div v-else-if="selectedLanguage === 'tr'">
      <h1>Filtrele</h1>

    </div>
        </button>
        <button class="button is-success is-light" @click="loadsortTodosComponent">


          <div v-if="selectedLanguage === 'en'">
      <h1>Sort Todos</h1>
    </div>

    <div v-else-if="selectedLanguage === 'tr'">
      <h1>Sırala</h1>

    </div>
          </button>
      </div>



      <div class="component-container">
        <component :is="currentComponent" :Select="selectedLanguage" />
      </div>
      </div>
    </div>
  </div>
</template>

<script>
import MainComponent from './components/Main.vue';
import TodosComponent from './components/GetTodo.vue';
import filterTodosComponent from './components/filterTodos.vue';
import sortTodosComponent from './components/sortTodos.vue';
import Preloader from './components/preloader.vue';


export default {
  name: 'App',
  components: {
    MainComponent,
    TodosComponent,
    filterTodosComponent,
    sortTodosComponent,
    Preloader,
  },
  data() {
    return {
      selectedLanguage: 'en',
      currentComponent: null,
      isLoading: true, // Şu anki yüklenen componenti tutar
    };
  },
  methods: {
    loadMainComponent() {
      this.currentComponent = 'MainComponent'; // FirstComponent yüklenecek
    },
    loadTodosComponent() {
      this.currentComponent = 'TodosComponent'; // SecondComponent yüklenecek
    },
    loadfilterTodosComponent() {
      this.currentComponent = 'filterTodosComponent'; // SecondComponent yüklenecek
    },
    loadsortTodosComponent() {
      this.currentComponent = 'sortTodosComponent'; // SecondComponent yüklenecek
    },
    changeLanguage(language) {
      this.selectedLanguage = language;
    },
  },
  mounted() {
    // Simüle edilen yükleme süreci
    setTimeout(() => {
      this.isLoading = false; // Yükleme tamamlandığında preloader'ı gizle
    }, 2000); // Örneğin, 3 saniye bekleyin ve daha sonra preloader'ı kaldırın
  },
};
</script>

<style>
.centered-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
 /* Sayfanın en az yüksekliği kadar container yüksekliği */
}

.button-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.button-container button {
  margin: 0 10px; /* Buttonlar arasında boşluk bırakma */
}

.image-container {
  margin-top: 20px;
  margin-bottom: 20px; /* Resim ile buttonlar arasında boşluk bırakma */
  text-align: center; /* Resmi yatayda ortala */
}

.component-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}
.button-image {
  width: 100px; /* Buttonların genişliği */
  height: 40px; /* Buttonların yüksekliği */
}
.content-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}
</style>


