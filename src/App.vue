<template>
  <div id="app">
    <button @click="loadMainComponent">Main Page</button>
    <button @click="loadTodosComponent">Get Todos</button>
    <button @click="loadfilterTodosComponent">Filter Todos</button>
    <button @click="loadsortTodosComponent">Sort Todos</button>

    <component :is="currentComponent" /> <!-- Dinamik olarak component yüklemesi -->
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
/* Stil tanımları */
</style>


