<script lang="ts">
	export let values: (string | number)[][];
	let percs: { row: number; col: number }[][] = [[]];

	const m = values.length,
		n = values[0].length;

	for (let i = 1; i < m; i++) {
		values[i].push(
			values[i].slice(1).reduce((acc, i) => {
				return Number(acc) + Number(i);
			}, 0)
		);
	}

	values[0].push('Total');
	values.push(['Total']);

	for (let j = 1; j <= n; j++) {
		let sum = 0;
		for (let i = 1; i < m; i++) {
			const x = Number(values[i][j]);
			sum += x;
		}
		values[values.length - 1].push(sum);
	}

	console.log(values);

	for (let i = 0; i < m - 1; i++) {
		percs.push([]);
		const rowMax = Number(values[i + 1][n]);
		for (let j = 0; j < n - 1; j++) {
			const colMax = Number(values[m][j + 1]);

			percs[i].push({
				row: (Number(values[i + 1][j + 1]) / rowMax) * 100,
				col: (Number(values[i + 1][j + 1]) / colMax) * 100
			});
		}
	}

	console.log(percs);

	let hover: boolean[][] = [];

	for (let i = 0; i < values.length; i++) {
		hover.push(new Array(values[0].length).fill(false));
	}
</script>

<div class="flex flex-col items-center pt-12">
	<div
		class="grid values border border-red-600 text-xl"
		style={`--rows: ${values.length}; --cols: ${values[1].length}`}
	>
		{#each values as row, i}
			{#each row as item, j}
				<span
					class="bg-red-100 p-4 text-center border border-red-600"
					on:mouseover={() => (hover[i][j] = true)}
					on:mouseout={() => (hover[i][j] = false)}
				>
					{item}
					{#if i > 0 && i < m && j > 0 && j < n && hover[i][j]}
						<div
							class="flex flex-col fixed z-10 p-5 border border-red-600 bg-white rounded-md text-sm"
						>
							<span>Row: {percs[i - 1][j - 1].row.toString().substring(0, 5)}%</span>
							<span>Col: {percs[i - 1][j - 1].col.toString().substring(0, 5)}%</span>
						</div>
					{/if}
				</span>
			{/each}
		{/each}
	</div>
</div>

<style>
	.values {
		grid-template-rows: repeat(var(--rows), minmax(0, 1fr));
		grid-template-columns: repeat(var(--cols), minmax(0, 1fr));
	}
</style>
