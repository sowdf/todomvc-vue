<template>
  <section class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <input
        autofocus="autofocus"
        autocomplete="off"
        placeholder="What needs to be done?"
        class="new-todo"
        v-model="newTodo"
        v-on:keydown.enter="add"
      />
    </header>

    <section class="main" v-show="todos.length">
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
          v-for="todo in filterTodos"
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
    <footer class="footer" v-show="todos.length">
      <span class="todo-count"
        ><strong>{{ selectedCount }}</strong> item left
      </span>
      <ul class="filters">
        <li><a href="#/all" :class="{ selected: hash === 'all' }">All</a></li>
        <li>
          <a href="#/active" :class="{ selected: hash === 'active' }">Active</a>
        </li>
        <li>
          <a href="#/completed" :class="{ selected: hash === 'completed' }"
            >Completed</a
          >
        </li>
      </ul>
      <button class="clear-completed" style="" @click="clearCompleted">
        Clear completed
      </button>
    </footer>
  </section>
</template>

<script>
const STORAGE_KEY = "todos-vuejs";

const todoStorage = {
  fetch: function() {
    return JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
  },
  save: function(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  }
};
const filters = {
  all: todos => {
    return todos;
  },
  active: todos => {
    return todos.filter(todo => !todo.completed);
  },
  completed: todos => {
    return todos.filter(todo => todo.completed);
  }
};
export default {
  name: "Todos",
  data() {
    return {
      hash: "all",
      newTodo: "",
      todos: todoStorage.fetch()
    };
  },
  watch: {
    todos: {
      deep: true,
      handler: todoStorage.save
    }
  },
  created() {
    const vm = this;
    vm.hash = location.hash.replace("#/", "");
    window.addEventListener("hashchange", () => {
      vm.hash = location.hash.replace("#/", "");
    });
  },
  computed: {
    selectedCount: function() {
      return filters.active(this.todos).length;
    },
    filterTodos: function() {
      return filters[this.hash](this.todos);
    },
    allDone: {
      get: function() {
        return this.selectedCount === 0;
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
