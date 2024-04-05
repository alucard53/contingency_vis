<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	let rows: number | null = null,
		cols: number | null = null;

	const dispatch = createEventDispatcher();

	function handleSubmit() {
		if (rows && cols) {
			dispatch('dim_sum', {
				rows,
				cols
			});
		} else {
			alert('Enter rows and cols');
		}
	}
</script>

<form
	on:submit|preventDefault={handleSubmit}
	class="flex flex-col justify-center items-center gap-3"
>
	<p class="text-xl font-medium">Enter number of rows and columns in contingency table:</p>
	<label for="rows" class="w-1/5">
		<input
			type="number"
			placeholder="rows"
			bind:value={rows}
			min="0"
			class="text-center w-full h-12 rounded-lg border-2 border-red-600"
		/>
	</label>
	<label for="cols" class="w-1/5">
		<input
			type="number"
			placeholder="cols"
			bind:value={cols}
			min="0"
			class="text-center w-full h-12 rounded-lg border-2 border-red-600"
		/>
	</label>
	<button type="submit" class="bg-white py-1 px-4 rounded-md hover:bg-gray-100 transition-all"
		>Next</button
	>
</form>

<style>
	input::-webkit-outer-spin-button,
	input::-webkit-inner-spin-button {
		-webkit-appearance: none;
		margin: 0;
	}
</style>
