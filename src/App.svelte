<script>
  let items = [];
  let editing = null;

  items = JSON.parse(localStorage.getItem("todos-svelte")) || [];

  function newItem(e) {
    if (e.key === "Enter") {
      if (e.target.value.length > 0) {
        let item = {
          id: 4,
          description: e.target.value,
          completed: false,
        };
        items = [...items, item];
        e.target.value = "";
      }
    }
  }

  function remove(index) {
    items = items.filter((e,i) => i != index);
  }

  function handleEdit(e) {
    if (e.key === "Enter") {
      e.target.blur();
    } else if (e.key === "Escape") {
      e.target.value = items[editing].description;
      e.target.blur();
    }
  }

  function submit(e) {
    items[editing].description = e.target.value;
    editing = null;
  }

  $: localStorage.setItem("todos-svelte", JSON.stringify(items));
</script>

<header class="header">
  <h1>todos</h1>
  <input
    class="new-todo"
    placeholder="What needs to be done?"
    on:keydown={newItem}
  />
</header>

{#if items.length > 0}
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
            <input
              class="toggle"
              type="checkbox"
              bind:checked={item.completed}
            />
            <label on:dblclick={() => (editing = index)} for="description"
              >{item.description}</label
            >
            <button class="destroy" on:click={() => remove(index)} />
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
{/if}
