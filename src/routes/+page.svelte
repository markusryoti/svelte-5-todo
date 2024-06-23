<script lang="ts">
	type Todo = {
		id: string
		name: string
		done: boolean
	}
	
	let todos = $state<Todo[]>([
		{id: newId(), name: "first", done: false},
		{id: newId(), name: "second", done: false},
		{id: newId(), name: "third", done: false}
	]);

	let newTodoInput = $state("")

	function newId() {
		return window.crypto.randomUUID()
	}

	function addTodo() {
        newTodoInput.valueOf()
		todos.push({id: newId(), name: newTodoInput.valueOf(), done: false})
		newTodoInput = ""
	}

	function removeTodo(todoId: string) {
		todos = todos.filter(t => t.id !== todoId)
	}

	function toggleDone(todoId: string) {
		todos = todos.map(t => {
			if (t.id === todoId) {
				t.done = !t.done
			}
			return t
		})
	}
</script>

<div>
	<input placeholder="new todo" bind:value={newTodoInput} />
	<button onclick={addTodo}>add</button>
</div>

<ul>
	{#each todos as {id, name, done}}
		<li class="todo-row">
			<span aria-hidden=true onclick={() => toggleDone(id)} class={done ? "done" : ""}>{name}</span>
			<button onclick={() => removeTodo(id)}>X</button>		
		</li>
	{/each}
</ul>

<style>
	ul {
		padding: 1rem 0;
	}
	
	.todo-row {
		display: flex
	}

	.todo-row span {
		padding: 0 0.5rem;
	}

	.done {
        text-decoration: line-through;
	}
</style>