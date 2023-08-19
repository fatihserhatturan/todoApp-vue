<template>
  <div id="app">
    <div class="centered-container">

      <div class="image-container">
        <img
          src=".\images\todo.gif"
          class="button-image"
        />
      </div>

      <div class="content-container">
      <div class="button-container">
        <button class="button is-success is-light" @click="loadMainComponent">Main Page</button>
        <button class="button is-success is-light" @click="loadTodosComponent">Get Todos</button>
        <button class="button is-success is-light" @click="loadfilterTodosComponent">Filter Todos</button>
        <button class="button is-success is-light" @click="loadsortTodosComponent">Sort Todos</button>
      </div>



      <div class="component-container">
        <component :is="currentComponent" />
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

export default {
  name: 'App',
  components: {
    MainComponent,
    TodosComponent,
    filterTodosComponent,
    sortTodosComponent,
  },
  data() {
    return {
      currentComponent: null, // Şu anki yüklenen componenti tutar
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


