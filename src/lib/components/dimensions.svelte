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
	class="flex flex-col justify-center items-center gap-3 h-1/4"
>
	<p class="text-xl font-medium mb-8">Enter number of rows and columns in contingency table:</p>
	<label for="rows" class="w-1/6">
		<input
			type="number"
			placeholder="rows"
			bind:value={rows}
			min="1"
			class="text-center w-full h-12 rounded-lg border-2 border-red-600 text-2xl"
		/>
	</label>
	<label for="cols" class="w-1/6">
		<input
			type="number"
			placeholder="cols"
			bind:value={cols}
			min="1"
			class="text-center w-full h-12 rounded-lg border-2 border-red-600 text-2xl"
			required
		/>
	</label>
	<button
		type="submit"
		class="bg-white py-2 px-4 rounded-md hover:bg-red-50 transition-all border border-red-400 text-lg"
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
