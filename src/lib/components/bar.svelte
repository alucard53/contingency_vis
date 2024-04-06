<script lang="ts">
	export let values: (string | number)[][];

	import { Bar } from 'svelte-chartjs';
	import { Chart, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

	Chart.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

	const m = values.length;
	const n = values[0].length;

	console.log(values);

	const datasets = values.slice(1, m).map((row, index) => {
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
		datasets
	};

	console.log(datasets);
</script>

<div class="flex mt-16 w-1/2 h-1/2">
	<Bar {data} />
</div>
