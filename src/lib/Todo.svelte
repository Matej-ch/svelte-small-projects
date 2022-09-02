<script>
    import {fade, fly} from 'svelte/transition'

    let todos = [
        {id: 1, text: 'first todo', isCompleted: false, isEditing: false},
        {id: 2, text: 'second todo', isCompleted: true, isEditing: false}
    ]

    let currentFilter = 'all';

    let todoText = '';

    const addTodo = () => {
        todos = [...todos, {
            id: todos.length + 1,
            text: todoText
        }]
        todoText = '';
    }

    const editTodo = todo => () => {
        beforeEditCache = todo.text;
        todo.isEditing = true;
        todos = todos;
    }

    const doneEdit = todo => () => {
        if (todo.text.trim().length === 0) {
            todo.text = beforeEditCache;
        }

        todo.isEditing = false;
        todos = todos;
    }

    const doneEditKeyDown = (event, todo) => () => {
        if (event.key === 'Enter') {

            if (todo.text.trim().length === 0) {
                todo.text = beforeEditCache;
            }

            todo.isEditing = false;
            todos = todos;
        }

        if (event.key === 'Escape') {
            todo.isEditing = false;
            todo.text = beforeEditCache;
            todos = todos;
        }
    }

    const deleteTodo = id => () => {
        todos = todos.filter(todo => todo.id !== id)
    }

    const checkAll = () => {
        todos = todos.filter(todo => todo.isCompleted)
    }

    const clearCompleted = () => {
        todos = todos.filter(todo => !todo.isCompleted)
    }

    let beforeEditCache = '';

    $: remainingTodos = todos.filter(todo => !todo.isCompleted)

    $: filteredTodos = currentFilter === 'all' ? todos : currentFilter === 'active' ? todos.filter(todo => !todo.isCompleted) : todos.filter(todo => todo.isCompleted);

</script>

<div class="p-8">


    <form action="#" on:submit|preventDefault={addTodo} class="pb-4">
        <div>
            <input bind:value={todoText} placeholder="Enter todo text">
        </div>
    </form>

    {#if todos.length}
        <ul>
            {#each filteredTodos as todo (todo.id)}
                <li class="py-2 flex gap-2 items-center" in:fade out:fade={{duration:600}}>
                    <input type="checkbox" bind:checked={todo.isCompleted}>

                    {#if todo.isEditing}
                        <input type="text" bind:value={todo.text} on:blur={doneEdit(todo)}
                               on:keydown={doneEditKeyDown(event,todo)}>
                    {:else}
                        <span class:line-through={todo.isCompleted}>{todo.text}</span>
                    {/if}

                    <button on:click={editTodo(todo)}
                            class="{todo.isCompleted ? 'pointer-events-none opacity-60' : ''}">Edit
                    </button>
                    <button on:click={deleteTodo(todo.id)}>Delete</button>
                </li>
            {/each}
        </ul>

        <hr>
        <div class="py-2 flex justify-between">
            <button on:click={checkAll}>Check all</button>

            {#key remainingTodos}
                <div><span class="inline-block" in:fly={{y:-20}}>{remainingTodos.length}</span> items remaining</div>
            {/key}

        </div>

        <hr>

        <div class="py-2">
            <button on:click={() => currentFilter = 'all'}
                    class:border-amber-400={currentFilter === 'all'}>All
            </button>
            <button on:click={() => currentFilter = 'active'} class:border-amber-400={currentFilter === 'active'}>
                Active
            </button>
            <button on:click={() => currentFilter = 'completed'} class:border-amber-400={currentFilter === 'completed'}>
                Completed
            </button>
            <button on:click={clearCompleted}>Clear completed</button>

        </div>
    {:else}
        <div>No todos</div>
    {/if}


</div>