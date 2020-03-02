<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>
    <div class="main">
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :key="todo.name" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">
            <input type="checkbox" class="toggle" v-model="todo.completed">
            <label @dblclick="editTodo(todo)">{{todo.name}}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.name" @keyup.enter="editDone" @keyup.esc="cancelEdit" v-focus="todo === editing">
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="todos.length">
      <span class="todo-count"><strong>{{todoCount}}</strong> tâche(s) à faire</span>
      <ul class="filters">
        <li>
          <a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter ='all'">Toutes les tâches</a>
          <a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter ='todo'">Tâches à faire</a>
          <a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter ='done'">Tâches faites</a>
        </li>
      </ul>
      <button class="clear-completed" v-show="completed" @click.prevent="deleteCompletedTodos">Supprimer les tâches terminées</button>
    </footer>
  </section>
</template>

<script>
import Vue from "vue"

export default {
  data () {
    return {
      allDone: false,
      todos: [],
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },
  methods: {
    addTodo() {
      this.todos.push({
        name: this.newTodo,
        completed: false
      })
      this.newTodo = ''
    },
    deleteTodo(todo) {
      this.todos = this.todos.filter(element => element !== todo)
    },
    deleteCompletedTodos() {
      this.todos = this.todos.filter(todo => !todo.completed)
    },
    editTodo(todo) {
      this.editing = todo
      this.oldTodo = todo.name
    },
    editDone () {
      this.editing = null
    },
    cancelEdit () {
      this.editing.name = this.oldTodo
      this.editDone()
    }
  },
  computed: {
    todoCount() {
      return this.todos.filter(todo => !todo.completed).length
    },
    filteredTodos() {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
    completed() {
      return this.todos.filter(todo => todo.completed).length
    }
  },
  directives: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(() => {
          el.focus()
        })
      }
    }
  }
}

</script>

<style src="./todos.css"></style>
