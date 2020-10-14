<template>
  <section class="todoapp">
    <header class="header"><h1>todos</h1>
      <input autofocus="autofocus" autocomplete="off"
                                                 placeholder="What needs to be done?" class="new-todo"></header>
    <section class="main" style=""><input id="toggle-all" type="checkbox" class="toggle-all">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li class="todo">
          <div class="view">
            <input type="checkbox" class="toggle" />
            <label>231</label>
            <button class="destroy"></button>
          </div>
          <input type="text" class="edit" />
        </li>
      </ul>
    </section>
    <footer class="footer" style=""><span class="todo-count"><strong>1</strong> item left
				</span>
      <ul class="filters">
        <li><a href="#/all" class="selected">All</a></li>
        <li><a href="#/active" class="">Active</a></li>
        <li><a href="#/completed" class="">Completed</a></li>
      </ul>
      <button class="clear-completed" style="">
        Clear completed
      </button>
    </footer>
  </section>
</template>

<script>
export default {
  name: 'Todos'
}
</script>

<style src="./todo.css"></style>
