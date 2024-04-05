<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	export let rows: number;
	export let cols: number;

	const dispatch = createEventDispatcher();

	const values: (string | number)[][] = [];

	for (let i = 0; i <= rows; i++) {
		const row: string[] = [];
		for (let j = 0; j <= cols; j++) {
			row.push('');
		}
		values.push(row);
	}
	values[0][0] = '/';

	function getPlaceholder(i: number, j: number) {
		if (j == 0) {
			return 'Enter row name';
		} else if (i == 0) {
			return 'Enter col name';
		}
		return '';
	}

	function handleSubmit() {
		for (let i = 1; i <= rows; i++) {
			for (let j = 1; j <= cols; j++) {
				const n = Number(values[i][j]);
				if (isNaN(n)) {
					alert('non-numeric value entered');
					return;
				} else {
					values[i][j] = n;
				}
			}
		}

		dispatch('val_sum', {
			values
		});
	}
</script>

<div class="flex flex-col justify-center items-center gap-3 h-2/4">
	<p class="text-xl font-medium">Enter the values:</p>
	<form on:submit|preventDefault={handleSubmit} class="flex flex-col items-center gap-5">
		<div
			class="grid values border border-red-600"
			style={`--rows: ${rows + 1}; --cols: ${cols + 1}`}
		>
			{#each values as row, i}
				{#each row as _, j}
					{#if i == 0 && j == 0}
						<span class="bg-red-600"></span>
					{:else}
						<label>
							<input
								type="text"
								bind:value={values[i][j]}
								class="border border-red-600 text-center h-10"
								placeholder={getPlaceholder(i, j)}
								required
							/>
						</label>
					{/if}
				{/each}
			{/each}
		</div>
		<button type="submit" class="bg-white py-1 px-4 rounded-md hover:bg-gray-100 transition-all">
			Next
		</button>
	</form>
</div>

<style>
	.values {
		grid-template-rows: repeat(var(--rows), minmax(0, 1fr));
		grid-template-columns: repeat(var(--cols), minmax(0, 1fr));
	}
</style>
