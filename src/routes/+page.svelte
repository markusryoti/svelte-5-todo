<script lang="ts">
	type Todo = {
		id: string;
		name: string;
		done: boolean;
	};

	let todos = $state<Todo[]>([]);
	let newTodoInput = $state('');
	let isMounted = false;

	$inspect(todos);

	$effect(() => {
		if (!isMounted) {
			const existingTodos = JSON.parse(localStorage.getItem('todos') ?? '');
			if (existingTodos) {
				todos = existingTodos;
			} else {
				todos = getInitialTodos();
			}
		}
	});

	$effect(() => {
		localStorage.setItem('todos', JSON.stringify(todos));
	});

	function newId() {
		return window.crypto.randomUUID();
	}

	function addTodo(e: KeyboardEvent) {
		if (e.key !== 'Enter') return;
		todos.push({ id: newId(), name: newTodoInput, done: false });
		newTodoInput = '';
	}

	function removeTodo(todoId: string) {
		todos = todos.filter((t) => t.id !== todoId);
	}

	function toggleDone(todoId: string) {
		todos = todos.map((t) => {
			if (t.id === todoId) {
				t.done = !t.done;
			}
			return t;
		});
	}

	function getInitialTodos() {
		return [
			{ id: newId(), name: 'first', done: false },
			{ id: newId(), name: 'second', done: false },
			{ id: newId(), name: 'third', done: false }
		];
	}
</script>

<div>
	<div class="new-todo">
		<label for="add">Add a todo</label>
		<input name="add" placeholder="new todo" bind:value={newTodoInput} onkeydown={addTodo} />
	</div>

	<ol>
		{#each todos as { id, name, done }}
			<li class="todo-row">
				<span aria-hidden="true" onclick={() => toggleDone(id)} class={done ? 'done' : ''}
					>{name}</span
				>
				<button onclick={() => removeTodo(id)}>X</button>
			</li>
		{/each}
	</ol>
</div>

<style>
	.new-todo {
		display: flex;
		flex-direction: column;
		width: 500px;
	}

	.new-todo label {
		font-weight: 600;
		color: rgb(61, 61, 61);
	}

	.new-todo input {
		padding: 0.5rem;
	}

	ol {
		padding: 1rem 0;
	}

	.todo-row {
		list-style-position: inside;
	}

	.todo-row span {
		padding: 0 0.5rem;
	}

	.done {
		text-decoration: line-through;
	}
</style>
