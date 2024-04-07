<script lang="ts">
	import Bar from '$lib/components/bar.svelte';

	export let values: (string | number)[][];
	let percs: { row: number; col: number; total: number }[][] = [[]];

	const m = values.length,
		n = values[0].length;

	const default_values = [...values];

	// calc row total
	for (let i = 1; i < m; i++) {
		values[i].push(
			values[i].slice(1).reduce((acc, i) => {
				return Number(acc) + Number(i);
			}, 0)
		);
	}

	values[0].push('Total');
	values.push(['Total']);

	// calc col total
	for (let j = 1; j <= n; j++) {
		let sum = 0;
		for (let i = 1; i < m; i++) {
			const x = Number(values[i][j]);
			sum += x;
		}
		values[values.length - 1].push(sum);
	}

	// cacl percentages
	const totalMax = Number(values[m][n]);
	for (let i = 0; i < m - 1; i++) {
		percs.push([]);
		const rowMax = Number(values[i + 1][n]);
		for (let j = 0; j < n - 1; j++) {
			const colMax = Number(values[m][j + 1]);
			const curr = Number(values[i + 1][j + 1]);

			percs[i].push({
				row: (curr / rowMax) * 100,
				col: (curr / colMax) * 100,
				total: (curr / totalMax) * 100
			});
		}
	}

	// fill percentage hover box array
	let hover: boolean[][] = [];
	for (let i = 0; i < values.length; i++) {
		hover.push(new Array(values[0].length).fill(false));
	}
</script>

<div class="flex flex-col items-center pt-12 h-full">
	<div
		class="grid values border border-red-600 text-xl"
		style={`--rows: ${values.length}; --cols: ${values[1].length}`}
	>
		{#each values as row, i}
			{#each row as item, j}
				{#if i == 0 && j == 0}
					<span class="bg-red-600"></span>
				{:else}
					<span
						class="bg-red-100 p-4 text-center border border-red-600 hover:bg-red-200 transition-all"
						role="button"
						tabindex="0"
						on:mouseover={() => (hover[i][j] = true)}
						on:mouseout={() => (hover[i][j] = false)}
						on:blur={() => {}}
						on:focus={() => {}}
					>
						{item}
						{#if i > 0 && i < m && j > 0 && j < n}
							<div
								class="flex flex-col fixed z-10 p-5 border border-red-600 bg-white rounded-md bg-opacity-80 transition-opacity ease-in-out"
								class:hidden={!hover[i][j]}
							>
								<span>Row: {percs[i - 1][j - 1].row.toString().substring(0, 5)}%</span>
								<span>Col: {percs[i - 1][j - 1].col.toString().substring(0, 5)}%</span>
								<span>Total: {percs[i - 1][j - 1].total.toString().substring(0, 5)}%</span>
							</div>
						{/if}
					</span>
				{/if}
			{/each}
		{/each}
	</div>
	<Bar values={default_values} />
</div>

<style>
	.values {
		grid-template-rows: repeat(var(--rows), minmax(0, 1fr));
		grid-template-columns: repeat(var(--cols), minmax(0, 1fr));
	}
</style>
