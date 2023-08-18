<template>
  <div id="app">
    <button @click="loadMainComponent">Main Page</button>
    <button @click="loadTodosComponent">Get Todos</button>
    <button @click="loadfilterTodosComponent">Filter Todos</button>

    <component :is="currentComponent" /> <!-- Dinamik olarak component yüklemesi -->
  </div>
</template>

<script>
import MainComponent from './components/Main.vue';
import TodosComponent from './components/GetTodo.vue';
import filterTodosComponent from './components/filterTodos.vue';

export default {
  name: 'App',
  components: {
    MainComponent,
    TodosComponent,
    filterTodosComponent,
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
  },
};
</script>

<style>
/* Stil tanımları */
</style>


