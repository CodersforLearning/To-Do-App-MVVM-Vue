<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('');

const input_content = ref('')

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt = a.createdAt;
}))

const addTodo = () => {
  if (input_content.value.trim() === '') return;

  todos.value.push({
    content: input_content.value,
    createdAt: new Date().getTime()
  })
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>
  <main class="app">
    <h1>Welcome to the Vue ToDo List!</h1>

    <section>
      <h3>CREATE A TODO </h3>
      
      <form @submit.prevent="addTodo" class="input-form">
        <h4>What's on your todo list?</h4>
        <input 
          type="text" 
          placeholder="e.g. run an MVVM workshop"
          v-model="input_content">

          <input type="submit" value="Add to list">
      </form>
    </section>

    <section class="todo-list">
      <h3>TO DO LIST:</h3>
      <div class="list">
        <div v-for="todo in todos_asc" :class="'todo-item'">
          {{ todo.content }}
        </div>
      </div>
    </section>

  </main>
</template>
