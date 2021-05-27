<script>
  let editing = null;
  let items = [
    {
      id: 1,
      description: "Todo 1",
      completed: false,
    },
    {
      id: 2,
      description: "Todo 2",
      completed: true,
    },
  ];
</script>

<header class="header">
  <h1>todos</h1>
  <input class="new-todo" placeholder="What needs to be done?" />
</header>

<section class="main">
  <input id="toggle-all" class="toggle-all" type="checkbox" />
  <label for="toggle-all">Mark all as complete</label>

  <ul class="todo-list">
    {#each items as item, index}
      <li class="{item.completed ? 'completed' : ''}">
        <div class="view">
          <input class="toggle" type="checkbox" bind:checked={item.completed}>
          <label for="description">{item.description}</label>
          <button class="destroy" />
        </div>
      </li>
    {/each}
    <li>
      <div class="view">
        <input class="toggle" type="checkbox" />
        <label>Todo 1</label>
        <button class="destroy" />
      </div>
    </li>
    <li class="completed">
      <div class="view">
        <input class="toggle" type="checkbox" checked />
        <label>Todo 2</label>
        <button class="destroy" />
      </div>
    </li>
    <li class="editing">
      <div class="view">
        <input class="toggle" type="checkbox" />
        <label>Todo 3</label>
        <button class="destroy" />
      </div>
      <input value="Todo 3" id="edit" class="edit" />
    </li>
  </ul>

  <footer class="footer">
    <span class="todo-count">
      <strong>2</strong>
      items left
    </span>

    <ul class="filters">
      <li>
        <a class="selected" href="#/">All</a>
      </li>
      <li>
        <a class="" href="#/active">Active</a>
      </li>
      <li>
        <a class="" href="#/completed">Completed</a>
      </li>
    </ul>

    <button class="clear-completed">
      Clear completed
    </button>
  </footer>
</section>
