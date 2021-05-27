<script>
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
  let editing = null

  function handleEdit(e) {
    if(e.key === 'Enter') {
      e.target.blur()
    } else if(e.key === 'Escape') {
      e.target.value = items[editing].description
      e.target.blur()
    }
  }

  function submit(e) {
    items[editing].description = e.target.value
    editing = null
  }
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
      <li
        class="{item.completed ? 'completed' : ''} {editing === index
          ? 'editing'
          : ''}"
      >
        <div class="view">
          <input class="toggle" type="checkbox" bind:checked={item.completed} />
          <label on:dblclick={() => (editing = index)} for="description"
            >{item.description}</label
          >
          <button class="destroy" />
        </div>
        {#if editing === index}
          <input
            value={item.description}
            id="edit"
            class="edit"
            on:keydown={handleEdit}
            on:blur={submit}
          />
        {/if}
      </li>
    {/each}
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

    <button class="clear-completed"> Clear completed </button>
  </footer>
</section>
