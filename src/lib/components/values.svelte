<script lang="ts">
	export let rows: number;
	export let cols: number;

	console.log(rows, cols);
	const values: string[][] = [];

	for (let i = 0; i <= rows; i++) {
		const row: string[] = [];
		for (let j = 0; j <= cols; j++) {
			row.push('');
		}
		values.push(row);
	}

	function getPlaceholder(i: number, j: number) {
		if (i == 0 && j == 0) {
			return '/';
		} else if (j == 0) {
			return 'Enter row name';
		} else if (i == 0) {
			return 'Enter col name';
		}
		return '';
	}
</script>

<div class="flex flex-col justify-center items-center gap-3">
	<p class="text-xl font-medium">Enter the values:</p>
	<div class={`grid gr${rows + 1} gc${cols + 1}`}>
		{#each values as row, i}
			{#each row as _, j}
				<label>
					<input
						type="text"
						bind:value={values[i][j]}
						class="border border-black"
						placeholder={getPlaceholder(i, j)}
					/>
				</label>
			{/each}
		{/each}
	</div>
</div>

<style>
	.gr1 {
		grid-template-rows: repeat(3, minmax(0, 1fr));
	}
	.gr2 {
		grid-template-rows: repeat(3, minmax(0, 1fr));
	}
	.gr3 {
		grid-template-rows: repeat(3, minmax(0, 1fr));
	}
	.gr4 {
		grid-template-rows: repeat(3, minmax(0, 1fr));
	}
	.gr5 {
		grid-template-rows: repeat(3, minmax(0, 1fr));
	}
	.gr6 {
		grid-template-rows: repeat(3, minmax(0, 1fr));
	}
	.gc1 {
		grid-template-columns: repeat(1, minmax(0, 1fr));
	}
	.gc2 {
		grid-template-columns: repeat(2, minmax(0, 1fr));
	}
	.gc3 {
		grid-template-columns: repeat(3, minmax(0, 1fr));
	}
	.gc4 {
		grid-template-columns: repeat(4, minmax(0, 1fr));
	}
	.gc5 {
		grid-template-columns: repeat(5, minmax(0, 1fr));
	}
	.gc6 {
		grid-template-columns: repeat(6, minmax(0, 1fr));
	}
</style>
