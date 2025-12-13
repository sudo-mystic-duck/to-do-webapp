<script>
    import { onMount } from 'svelte';

    // define reactive variables
    let todos = $state([]);
    let input = $state("");

    // add todos
    function addTodo() {
        if(input === "") {
            alert("Please enter some text");
        } else {
            todos.push({
                text: input,
                done: false
            })
            input = "";
            localStorage.setItem("todolist", JSON.stringify(todos));
        }
    }

    // delete all todos
    function removeAllTodos() {
        let confirmation = confirm("Are you sure you want to remove all items?");
        if(confirmation) {
            todos = [];
            localStorage.setItem("todolist", JSON.stringify(todos));
        }
    }

    // remove checked todos
    function removeTodo() {
        todos = todos.filter((item) => !item.done);
        localStorage.setItem("todolist", JSON.stringify(todos));
    }

    onMount(() => {
        const storedTodos = localStorage.getItem("todolist");
        todos = storedTodos ? JSON.parse(storedTodos) : [];
    })
</script>



<div class="h-screen bg-linear-to-tl from-blue-700 dark:from-blue-800 via-purple-400 dark:via-purple-800 to-red-400 dark:to-red-800">
    <h1
            class="text-6xl font-bold flex justify-center dark:text-white"
    >To Do List</h1>

    <div class="flex justify-center space-x-1">
        <input
                bind:value={input}
                type="text"
                placeholder="Write something..."
                class="bg-white border-2 border-gray-800 hover:bg-gray-400 p-1 rounded-2xl"
        >

        {#if (input)}
            <button
                    onclick={addTodo}
                    class="bg-white text-black border-2 border-gray-700 p-1 rounded-2xl"
            >Add</button>
        {:else}
            <button
                    onclick={addTodo}
                    class="bg-gray-400 text-black border-2 border-gray-700 p-1 rounded-2xl"
                    disabled
            >Add</button>
        {/if}

        {#if (todos.some((item) => item.done))}
            <button
                    onclick={removeTodo}
                    class="bg-white text-black border-2 border-gray-700 p-1 rounded-2xl"
            >Remove</button>
        {:else}
            <button
                    onclick={removeTodo}
                    class="bg-gray-400 text-black border-2 border-gray-700 p-1 rounded-2xl"
                    disabled
            >Remove</button>
        {/if}

        {#if (todos.length >= 1)}
            <button
                    onclick={removeAllTodos}
                    class="bg-white text-black border-2 border-gray-700 p-1 rounded-2xl"
            >Remove All</button>
        {:else}
            <button
                    onclick={removeAllTodos}
                    class="bg-gray-400 text-black border-2 border-gray-700 p-1 rounded-2xl"
                    disabled
            >Remove All</button>
        {/if}
    </div>

    <div
            class="flex justify-center m-1 p-3.5 h-4/5 overflow-y-auto bg-linear-to-tr
            from-blue-700 dark:from-blue-800 via-purple-400 dark:via-purple-800
            to-red-400 dark:to-red-800 border-4 border-gray-700 rounded-2xl"
    >
        <ul class="w-4/5">
            {#each todos as todo}
                <li class="bg-white border-2 border-gray-700 p-1 rounded-2xl break-after-auto overflow-x-auto">
                    <label>
                        <input bind:checked={todo.done} type="checkbox" />
                        {todo.text}
                    </label>
                </li>
            {/each}
        </ul>
    </div>
    <a
            href="https://www.flaticon.com/free-icons/to-do-list"
            class="bg-white hover:bg-black border-2 text-black hover:text-white border-gray-700 hover:border-gray-400
            p-1 rounded-2xl font-medium fixed bottom-1 left-1/2 -translate-x-1/2 transition"
    >To do list icon created by Graphics Plazza - Flaticon</a>
</div>