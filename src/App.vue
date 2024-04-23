<template>
  <div class="container flex flex-col items-center gap-5 mt-8">
  <div class="text-4xl font-bold text-sky-500">Todo App</div>
  <div class="flex flex-col gap-2 w-1/2">
  <div class="flex gap-4" v-for="todo in todos" :key="todo.id">
    <input class="py-3 px-4 block w-full rounded-lg" type="text" :value="todo.todo" @input="updateTodo($event, todo.id)"/>
    <button @click="removeTodo(todo.id)">
      <font-awesome-icon :icon="['fas', 'x']" class="text-sky-500"/>
    </button>
  </div>
  </div>
  <div class="flex gap-4">
    <input class="py-3 px-4 block w-full rounded-lg" type="text" v-model="todo" placeholder="What needs to be done?" autofocus>
    <button @click="addTodo" class="bg-sky-500 hover:bg-sky-700 text-white py-2 px-4 rounded">add</button>
    <button @click="clearAllTodos" class="bg-red-500 hover:bg-red-700 text-white py-2 px-4 rounded">clear</button>
  </div>
</div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      todo:'',
      todos: [],
    }
  },
  watch: {
    todos: function () {
      localStorage.setItem('todos', JSON.stringify(this.todos))
      console.log('sêt stetet')
    }
  },
  created(){
    if (localStorage.getItem('todos')){
      this.todos = JSON.parse(localStorage.getItem('todos'))
    }
    else{
      this.fetchData();
    }
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch('https://dummyjson.com/todo'); 
        const data = await response.json(); 
        this.todos = data.todos;
        console.log('dang fetch')
      } catch (error) {
        console.error('Lỗi khi gọi API:', error);
      }
    },
    addTodo() {
      if(this.todo.length === 0) {
        return;
      } else {
           const newTodo = {
             id: Date.now(),
             todo: this.todo,
           };
           this.todos = [...this.todos, newTodo];
      }
      console.log(this.todos.length)
      this.todo = ''
    },
    removeTodo(todoId) {
      this.todos = this.todos.filter((todo) => todo.id != todoId)
    },
    clearAllTodos() {
      this.todos = []
    },
    updateTodo(event, id) {
      const updatedTodo = this.todos.map((todo) => {
        if(id === todo.id) {
          return {...todo, todo: event.target.value}
        }
        return todo
      })
      this.todos = updatedTodo
    },
  }
}
</script>

<style>
</style>
