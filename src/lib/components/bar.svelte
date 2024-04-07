<script lang="ts">
	export let values: (string | number)[][];

	import { Bar } from 'svelte-chartjs';
	import { Chart, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

	Chart.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

	const m = values.length;
	const n = values[0].length;

	const transpose: (string | number)[][] = [];
	for (let i = 0; i < n - 1; i++) {
		transpose.push(new Array(m).fill(0));
	}

	for (let i = 0; i < m; i++) {
		for (let j = 0; j < n - 1; j++) {
			transpose[j][i] = values[i][j];
		}
	}

	const datasets_transpose = transpose.slice(1, n - 1).map((row, index) => {
		return {
			label: row[0],
			data: row.slice(1),
			backgroundColor: row
				.slice(1, m - 1)
				.map((_) => (index % 2 === 0 ? 'rgba(255, 134,159,0.4)' : 'rgba(98,  182, 239,0.4)'))
		};
	});

	const datasets_normal = values.slice(1, m).map((row, index) => {
		return {
			label: row[0],
			data: row.slice(1, n - 1),
			backgroundColor: row
				.slice(1, n - 1)
				.map((_) => (index % 2 === 0 ? 'rgba(255, 134,159,0.4)' : 'rgba(98,  182, 239,0.4)'))
		};
	});

	const data = {
		labels: values[0].slice(1, n - 1),
		datasets: datasets_normal
	};
</script>

<div class="flex flex-col pt-16 w-1/2 h-1/2 gap-10 items-center">
	<Bar {data} />
	<button
		class="bg-white py-2 px-4 rounded-md hover:bg-red-50 transition-all border border-red-400 text-lg"
		on:click={() => {
			data.labels =
				data.datasets === datasets_transpose ? values[0].slice(1, n - 1) : transpose[0].slice(1, m);
			data.datasets = data.datasets === datasets_transpose ? datasets_normal : datasets_transpose;
		}}>Invert</button
	>
</div>
