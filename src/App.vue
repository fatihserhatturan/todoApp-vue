<template>
  <div class="content">
    <form @submit.prevent="addTodo">
      <div class="field">
  <label class="label"> Todo Name</label>
  <div class="control">
    <input v-model="todo.name" class="input" type="text" placeholder="Text input">
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
      <select>
        <option>Select dropdown</option>
        <option>With options</option>
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
      <input v-model="todo.priority" type="radio" value="High" name="question">
      High
    </label>
    <label class="radio">
      <input v-model="todo.priority" type="radio" value="Mid" name="question">
      Mid
    </label>
    <label class="radio">
      <input v-model="todo.priority" type="radio" value="Low" name="question">
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
        <p :class="{done:todo.done}" @click="done(todo)" class="subtitle is-6">{{ todo.priority }}</p>
      </div>
    </div>

    <div class="content">
      {{ todo.description }}<a>Done: {{ todo.done }}</a>.

      <br>
      <time datetime="2016-1-1">11:09 PM - 1 Jan 2016</time>
    </div>
  </div>
</div>


  </div>

</template>

<script>
import{ref,reactive} from 'vue'
export default{
  setup(){
    //const todo = ref('')
    const todo = reactive({
      name: '',
      image: '',
      description: '',
      priority: '',
      category:''

    })
    const todos = ref([])

    function addTodo(){
      todos.value.push({
        done:false,
        description:todo.description,
        name:todo.name,
        image:todo.image,
        priority:todo.priority,
        category:todo.category,


      })

    }
    function done(todo){
      todo.done = !todo.done
    }

    return {
      todo,
      todos,
      addTodo,
      done
    }
  }

}
</script>

<style lang="scss">
.done {
  text-decoration: line-through;
}

</style>
