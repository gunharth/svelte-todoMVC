<script>
  import { v4 as uuidv4 } from "uuid";

  let items = [];
  let editing = null;
  let currentFilter = "all";

  items = JSON.parse(localStorage.getItem("todos-svelte")) || [];

  function newItem(event) {
    if (event.key === "Enter") {
      if (event.target.value.length > 0) {
        let item = {
          id: uuidv4(),
          description: event.target.value,
          completed: false,
        };
        items = [...items, item];
        event.target.value = "";
      }
    }
  }

  function remove(index) {
    items = items.filter((e, i) => i != index);
  }

  function handleEdit(event) {
    if (event.key === "Enter") {
      event.target.blur();
    } else if (event.key === "Escape") {
      event.target.value = items[editing].description;
      event.target.blur();
    }
  }

  function submit(event) {
    items[editing].description = event.target.value;
    editing = null;
  }

  function toggleAll(event) {
    items = items.map((item) => ({
      id: item.id,
      description: item.description,
      completed: event.target.checked,
    }));
  }

  function clearCompleted() {
    items = items.filter((item) => !item.completed);
  }

  function render() {
    currentFilter = "all";
    if (window.location.hash === "#active") {
      currentFilter = "active";
    } else if (window.location.hash === "#completed") {
      currentFilter = "completed";
    }
  }

  render();

  window.addEventListener("hashchange", render);

  $: filtered =
    currentFilter === "all"
      ? items
      : currentFilter === "completed"
      ? items.filter((item) => item.completed)
      : items.filter((item) => !item.completed);

  $: numActive = items.filter((item) => !item.completed).length;
  $: numCompleted = items.filter((item) => item.completed).length;

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
    <input
      id="toggle-all"
      class="toggle-all"
      type="checkbox"
      on:change={toggleAll}
      checked={numCompleted === items.length}
    />
    <label for="toggle-all">Mark all as complete</label>

    <ul class="todo-list">
      {#each filtered as item, index (item.id)}
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
        <strong>{numActive}</strong>
        {numActive === 1 ? "item" : "items"} left
      </span>

      <ul class="filters">
        <li>
          <a class={currentFilter === "all" ? "selected" : ""} href="#all">
            All
          </a>
        </li>
        <li>
          <a
            class={currentFilter === "active" ? "selected" : ""}
            href="#active"
          >
            Active
          </a>
        </li>
        <li>
          <a
            class={currentFilter === "completed" ? "selected" : ""}
            href="#completed"
          >
            Completed
          </a>
        </li>
      </ul>

      {#if numCompleted}
        <button class="clear-completed" on:click={clearCompleted}>
          Clear completed
        </button>
      {/if}
    </footer>
  </section>
{/if}
