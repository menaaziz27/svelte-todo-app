<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import type { TodoType } from '../types/todo.type';
	import { fade } from 'svelte/transition';

	const dispatch = createEventDispatcher();

	export let todo: TodoType;
</script>

<li transition:fade>
	<div>
		<input type="checkbox" checked={todo.completed} on:change={() => dispatch('toggleCompleted')} />
		<span class={'done-' + todo.completed}>{todo.title}</span>
	</div>
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<svg
		on:click={() => dispatch('delete')}
		xmlns="http://www.w3.org/2000/svg"
		width="1em"
		height="1em"
		viewBox="0 0 24 24"
		{...$$props}
		><path
			fill="currentColor"
			d="M6 19c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7H6v12zM19 4h-3.5l-1-1h-5l-1 1H5v2h14V4z"
		/></svg
	>
</li>

<style>
	.done-true {
		color: gray;
		text-decoration: line-through;
	}

	svg {
		cursor: pointer;
	}

	li {
		display: flex;
		justify-content: space-between;
		margin-top: 5px;
		font-size: 22px;
		font-weight: 500;
		border: 1px solid #ccc;
		border-radius: 4px;
		background-color: #fafafa;
		padding: 5px;
	}

	li:hover {
		border-color: #b9b1e9;
	}
</style>
