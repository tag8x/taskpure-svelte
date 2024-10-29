<script>
    import { onMount } from 'svelte';

    let todos = [];
    let newTodo = '';
    let isDarkMode = false;

    function addTodo() {
        if (newTodo.trim() !== '') {
            todos = [...todos, { text: newTodo, completed: false }];
            newTodo = '';
        }
    }

    function deleteTodo(index) {
        todos = todos.filter((_, i) => i !== index);
    }

    function toggleComplete(index) {
        todos[index].completed = !todos[index].completed;
        todos = [...todos];
    }

    function toggleDarkMode() {
        isDarkMode = !isDarkMode;
        localStorage.setItem('darkMode', JSON.stringify(isDarkMode));
    }

    onMount(() => {
        const savedDarkMode = JSON.parse(localStorage.getItem('darkMode'));
        if (savedDarkMode !== null) {
            isDarkMode = savedDarkMode;
        }
    });
</script>

<style>
    :global(body) {
        font-family: Arial, sans-serif;
        margin: 0;
        background-color: var(--background-color);
        color: var(--text-color);
        transition: background-color 0.3s ease, color 0.3s ease;
    }

    :root {
        --background-color: #f9f9f9;
        --text-color: #333;
    }

    .dark-mode {
        --background-color: #222;
        --text-color: #f9f9f9;
    }

    .container {
        max-width: 400px;
        margin: 2rem auto;
        padding: 1rem;
        border-radius: 8px;
        box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
    }

    h1 {
        text-align: center;
    }

    .todo-list {
        list-style: none;
        padding: 0;
    }

    .todo-item {
        display: flex;
        justify-content: space-between;
        padding: 0.5rem;
        margin-bottom: 0.5rem;
        border-radius: 5px;
        background: var(--background-color);
        transition: background-color 0.3s ease;
    }

    .todo-item.completed {
        text-decoration: line-through;
        opacity: 0.6;
    }

    .actions {
        display: flex;
        gap: 0.5rem;
    }

    .add-todo {
        display: flex;
        gap: 0.5rem;
    }

    input[type="text"] {
        flex: 1;
        padding: 0.5rem;
        border: 1px solid #ddd;
        border-radius: 5px;
    }

    button {
        padding: 0.5rem;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }

    .dark-mode-toggle {
        display: block;
        margin: 0 auto;
        margin-top: 1rem;
        background-color: #444;
        color: #fff;
    }
</style>

<div class:dark-mode={isDarkMode}>
    <div class="container">
        <h1>To-Do List</h1>

        <div class="add-todo">
            <input type="text" bind:value={newTodo} placeholder="Add new task..." />
            <button on:click={addTodo}>Add</button>
        </div>

        <ul class="todo-list">
            {#each todos as todo, index}
                <li class="todo-item {todo.completed ? 'completed' : ''}">
                    <button 
                        on:click={() => toggleComplete(index)} 
                        on:keydown={(e) => e.key === 'Enter' && toggleComplete(index)} 
                        style="background: none; border: none; color: inherit; text-align: left; cursor: pointer;">
                        {todo.text}
                    </button>
                    <div class="actions">
                        <button on:click={() => deleteTodo(index)}>Delete</button>
                    </div>
                </li>
            {/each}
        </ul>
        

        <button class="dark-mode-toggle" on:click={toggleDarkMode}>
            {isDarkMode ? 'Light Mode' : 'Dark Mode'}
        </button>
    </div>
</div>