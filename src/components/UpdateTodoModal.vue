<template>
    <div v-if="isOpen" class="modal is-active">
      <div class="modal-background"></div>
      <div class="modal-content">
        <h1>Name :</h1>
        <input v-model="updatedTodoName" class="input" type="text" :placeholder="todo.name">

        <h1>Category :</h1>
        <input v-model="updatedTodoName" class="input" type="text" :placeholder="todo.category" >

        <h1>Description :</h1>
        <input v-model="updatedTodoName" class="input" type="text" :placeholder="todo.description">

        <h1>İmage :</h1>
        <input v-model="updatedTodoName" class="input" type="text" :placeholder="todo.image">

        <h1>Importance :</h1>
        <input v-model="updatedTodoName" class="input" type="text" :placeholder="todo.importance">
        <button class="button is-success" @click="updateTodo" >Update</button>
      </div>
      <button class="modal-close is-large" aria-label="close" @click="closeModal"></button>
    </div>
  </template>

  <script>
  import { ref,computed } from 'vue';

  export default {
    props: {
      isOpen: Boolean,
      todo: Object,
      selectedTodoObject: Array,
      updateTodo: Function,
      closeModal: Function,
    },
    setup(props) {
      const updatedTodoName = ref('');

      function updateTodo() {
        props.updateTodo();
      }

      const todo = computed(() => {
      return props.selectedTodoObject;
    });

      function closeModal() {
       
        props.closeModal();
      }

      return {
        updatedTodoName,
        updateTodo,
        closeModal,
        selectedTodoObject: props.selectedTodoObject,
        todo,// SelectedTodoObject'u doğrudan props olarak alabilirsiniz
      };
    },
  };
  </script>


