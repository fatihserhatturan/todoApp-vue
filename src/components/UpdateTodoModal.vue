<template>
    <div v-if="isOpen" class="modal is-active">
      <div class="modal-background"></div>
      <div class="modal-content">
        <h1 class="modal-title">Name:</h1>
        <input v-model="todo.name" class="input" type="text" >

        <h1 class="modal-title">Description:</h1>
        <input v-model="todo.description" class="input" type="text" >

        <h1 class="modal-title">Last Day:</h1>
        <input v-model="todo.lastday" class="input" type="text" >

        <h1 class="modal-title">Image:</h1>
        <input v-model="todo.image" class="input" type="text" >

        <h1 class="modal-title">Importance:</h1>
        <label class="radio">
          <input v-model="todo.importance" type="radio" value="High" name="question">
          <span>{{ Select === 'en' ? 'High' : 'Yüksek' }}</span>
        </label>
        <label class="radio">
          <input v-model="todo.importance" type="radio" value="Mid" name="question">
          <span>{{ Select === 'en' ? 'Mid' : 'Orta' }}</span>
        </label>
        <label class="radio">
          <input v-model="todo.importance" type="radio" value="Low" name="question">
          <span>{{ Select === 'en' ? 'Low' : 'Düşük' }}</span>
        </label>

        <button class="button is-success" @click="updateTodo">Update</button>
      </div>
      <button class="modal-close is-large" aria-label="close" @click="closeModal"></button>
    </div>
  </template>

  <script>
  import { ref,reactive,computed } from 'vue';
  import axios from 'axios';

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
      const todo = computed(() => {
      return props.selectedTodoObject;
    });

     async function updateTodo() {

        try {
        const url ='http://localhost:5000/api/todo/'+todo.value._id;
        const response = await axios.put(url,{
            name:todo.value.name,
            description:todo.value.description,
            importance:todo.value.importance,
            category:todo.value.category,
            image:todo.value.image,
            lastday:todo.value.lastday,
        });

        } catch (error) {
        console.error(error)
       }
        props.updateTodo();
      }

      function closeModal() {
        console.log(todo.value.importance);
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

<style lang="scss">
.modal-title {
  font-size: 1.2rem;

  margin-bottom: 0.5rem;
  color: bisque;
}
.radio {
  display: flex;
  align-items: center;
  margin-top: 0.5rem;
}
.radio input {
  margin-right: 0.5rem;
}
</style>


