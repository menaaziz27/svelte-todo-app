<script lang="ts">
	import Todo from '$lib/Todo.svelte';
	import type { TodoType } from '../types/todo.type';
	import { browser } from '$app/environment';

	let todoTitle = '';

	let savedTodosString;
	if (browser) {
		savedTodosString = localStorage.getItem('todos');
	}

	let loadedTodos: TodoType[] = savedTodosString ? JSON.parse(savedTodosString) : [];
	$: noOfTodos = loadedTodos.length || 0;
	$: unCompletedTodos = loadedTodos.filter((t) => !t.completed).length;

	function toggleCompleted(id: number) {
		loadedTodos = loadedTodos.map((t) => (t.id === id ? { ...t, completed: !t.completed } : t));
		localStorage.setItem('todos', JSON.stringify(loadedTodos));
	}

	const createTodo = (title: string, completed = false) => ({ id: ++noOfTodos, title, completed });

	function handleSubmit() {
		if (!todoTitle) return;
		loadedTodos = loadedTodos.concat(createTodo(todoTitle));
		localStorage.setItem('todos', JSON.stringify(loadedTodos));
		todoTitle = '';
	}

	function deleteTodo(id: number) {
		loadedTodos = loadedTodos.filter((t) => t.id !== id);
		localStorage.setItem('todos', JSON.stringify(loadedTodos));
	}

	function deleteAll() {
		loadedTodos = [];
		localStorage.setItem('todos', JSON.stringify(loadedTodos));
	}
</script>

<main>
	<section class="content">
		<h1 class="heading">Todos</h1>
		<p>{unCompletedTodos} / {noOfTodos}</p>

		<form on:submit|preventDefault={handleSubmit}>
			<div class="form-control">
				<input
					type="text"
					id="title"
					name="title"
					bind:value={todoTitle}
					placeholder="Type new todo here"
				/>
				<button type="submit" class="create-btn">Create</button>
			</div>
		</form>
		<ul class="unstyled">
			{#if loadedTodos.length}
				{#each loadedTodos as todo}
					<Todo
						{todo}
						on:toggleCompleted={() => toggleCompleted(todo.id)}
						on:delete={() => deleteTodo(todo.id)}
					/>
				{/each}
			{/if}
		</ul>
		<button on:click={deleteAll} class="delete-all-btn">Delete All</button>
	</section>
</main>

<style>
	main {
		display: flex;
		justify-content: center;
		font-family: 'Courier New', Courier, monospace;
		padding-top: 100px;
	}

	.heading {
		font-size: 2rem;
		font-weight: 500;
		margin-bottom: 0;
	}

	.delete-all-btn {
		border: 1px solid #ccc;
		border-radius: 5px;
		padding: 5px;
		outline: 0;
		cursor: pointer;
		display: inline-block;
	}
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 10px;
		border: 1px solid #ccc;
		border-radius: 10px;
		padding: 10px;
		width: 100%;
		max-width: 600px;
	}

	form {
		width: 100%;
	}

	input {
		border: 1px solid #ccc;
		border-radius: 5px;
		padding: 5px 10px;
		outline: 0;
		font-size: 1rem;
		font-weight: 500;
	}

	input:focus {
		border-color: #b9b1e9;
	}

	button {
		border: 1px solid #ccc;
		border-radius: 5px;
		padding: 5px 10px;
		outline: 0;
		cursor: pointer;
		font-size: 1rem;
		font-family: inherit;
	}

	button:hover {
		border-color: #b9b1e9;
	}

	.form-control {
		display: flex;
		flex-direction: column;
		gap: 5px;
		width: 100%;
	}
	ul.unstyled {
		list-style: none;
		margin-left: 0;
		padding-left: 0;
		width: 100%;
	}

	@media (max-width: 480px) {
		main {
			padding-top: 30px;
		}

		.heading {
			font-size: 1.5rem;
		}
	}
</style>
