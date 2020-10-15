<template>
  <section class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <div>
        Todo <span>{{ newTodo }}</span>
      </div>
      <input
        autofocus="autofocus"
        autocomplete="off"
        placeholder="What needs to be done?"
        class="new-todo"
        v-model="newTodo"
        v-on:keydown.enter="add"
      />
    </header>

    <section class="main" style="">
      <input
        id="toggle-all"
        type="checkbox"
        class="toggle-all"
        v-model="allDone"
      />
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li
          class="todo"
          :class="{ completed: todo.completed }"
          v-for="todo in todos"
          v-bind:key="todo.id"
        >
          <div class="view">
            <input type="checkbox" class="toggle" v-model="todo.completed" />
            <label>{{ todo.title }}</label>
            <button class="destroy" v-on:click="remove(todo)"></button>
          </div>
          <input type="text" class="edit" />
        </li>
      </ul>
    </section>
    <footer class="footer" style="">
      <span class="todo-count"
        ><strong>{{ selectedCount }}</strong> item left
      </span>
      <ul class="filters">
        <li><a href="#/all" class="selected">All</a></li>
        <li><a href="#/active" class="">Active</a></li>
        <li><a href="#/completed" class="">Completed</a></li>
      </ul>
      <button class="clear-completed" style="" @click="clearCompleted">
        Clear completed
      </button>
    </footer>
  </section>
</template>

<script>
export default {
  name: "Todos",
  data() {
    return {
      newTodo: "",
      todos: []
    };
  },

  computed: {
    selectedCount: function() {
      return this.todos.filter(todo => todo.completed).length;
    },
    allDone: {
      get: function() {
        return "";
      },
      set: function(value) {
        this.todos.forEach(function(todo) {
          todo.completed = value;
        });
      }
    }
  },

  methods: {
    add() {
      this.todos.push({
        id: new Date().getTime(),
        completed: false,
        title: this.newTodo
      });
      this.newTodo = "";
    },
    remove(todo) {
      const index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    }
  }
};
</script>

<style src="./base.css"></style>
<style src="./index.css"></style>
