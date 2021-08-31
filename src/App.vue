<template>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous">
</head>
  <div class="container">
    <div class="header">
      <h1 class="name_header">Список дел</h1>
    </div>
    <div class="container_input">
      <input 
        class="form-control"
        type="text" 
        placeholder="Новое дело..." 
        id="exampleFormControlInput1" 
        v-model="title">
      <button id="myButton" class="btn btn-primary" @click="onSubmit">Добавить</button>
    </div>

    <my-list 
      :todos="filteredItems"
      @remove-todo="removeTodo"
      @storage-item="storageItem"
      @id-check="checkboxValue"
      @remove-click-button-item="removeTodo">
    </my-list>

    <div id="text_result">
      Всего задач:
      <span id="result">{{counterTask}}</span>
      <div class="filter_button">
        <button class="btn btn-primary" :class="{'active': currentFilter === 'all'}" @click="changeFilter('all')" >Все</button>
        <button class="btn btn-primary" :class="{'active': currentFilter === 'activ'}" @click="changeFilter('activ')">Активные</button>
        <button class="btn btn-primary" :class="{'active': currentFilter === 'carriedOut'}" @click="changeFilter('carriedOut')">Выполненные</button>
      </div>
    </div>
  </div>
</template>

<script>
import myList from './components/myList.vue'
export default {
  name: 'App',
  data() {
    return {
      todos: [],
      title: "",
      listname: "",   
      currentFilter: "all", 
      checked: false,
    }
  },

  mounted() {
    this.todos = JSON.parse(localStorage.getItem("title")) || []
  },

  methods: {
    storageItem (arr) {
      localStorage.setItem("title", JSON.stringify(arr));
    },
    
    addTodo(todo) {
      this.todos = [...this.todos, todo]
      this.storageItem(this.todos)
    },

    onSubmit() {
      if(this.title.trim()){
        const newTodo = {
          id: this.searchId + 1,
          title: this.title, 
          checked: this.checked,
        }
      this.addTodo(newTodo)
      this.storageItem(this.todos)
      this.title = ''
      } 
    },

    removeTodo(e) {
      const removeTodoIds = this.todos.findIndex(t => t.id === e.id)
      this.todos.splice(removeTodoIds, 1)
      this.storageItem(this.todos)
    },

    checkboxValue(e) {
      const newTodos = this.todos.findIndex(t => t.id === e.id)
      this.todos[newTodos].checked = e.value
      this.storageItem(this.todos)
      console.log(this.todos)
    },
    
    changeFilter(value) {
      this.currentFilter = value
    },
  },
  computed: {
    searchId() {
      const todoIds = this.todos.map(t => t.id)
      return Math.max(...todoIds, 0)
    },

    counterTask() {
      return this.filteredItems.length
    },

    filteredItems() {
      if(this.currentFilter === 'all') {
        return this.todos
      }
      if(this.currentFilter === 'activ'){
        return this.todos.filter(todo => !todo.checked)
      }
      if(this.currentFilter === 'carriedOut') {
        return this.todos.filter(todo => todo.checked)
      }
      return this.todos
    }
  },
  
  components: {
    myList
  }
}
</script>

<style>
h1 {
  font-family: "Roboto", cursive;
}

.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
  margin: 30px auto;
  width: 60%;
  height: 50%;
}

.container_input {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.btn {
  font-family: "Roboto", cursive;
  margin-left: 10px;
}

.name_header {
  font-family: "Roboto", cursive;
  margin: 10px;
}

#text_result {
  font-family: "Roboto", cursive;
  padding: 10px;
}

.form-control {
  font-family: "Roboto", cursive;
}

.done {
  text-decoration: line-through;
}
</style>
