<template>
    <div class="modal" :class="{ 'is-active': isOpen }">
      <div class="modal-background"></div>
      <div class="modal-content">
        <h1>Update</h1>
        <input v-model="updatedTodoName" class="input" type="text" placeholder="Updated Todo Name">
        <button class="button is-success" @click="updateTodo">Update</button>
      </div>
      <button class="modal-close is-large" aria-label="close" @click="closeModal"></button>
    </div>
  </template>

  <script>
  import { ref } from 'vue';

  export default {
    props: {
      isOpen: Boolean,
      todo: Object,
      updateTodo: Function,
      closeModal: Function,
    },
    setup(props) {
      const updatedTodoName = ref('');

      function updateTodo() {
        // Parent bileşenden alınan işlevi burada çağırabilirsiniz
        props.updateTodo();
      }

      function closeModal() {
        
        props.closeModal();
    }


      return {
        updatedTodoName,
        updateTodo,
        closeModal,
      };
    }
  };
  </script>

