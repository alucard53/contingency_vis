<script lang="ts">
	import Dimensions from '$lib/components/dimensions.svelte';
	import Display from '$lib/components/display.svelte';
	import Values from '$lib/components/values.svelte';

	enum State {
		DIM,
		VAL,
		DISP
	}
	let state = State.DIM;

	let dims = { rows: 0, cols: 0 };
	let values: (string | number)[][] = [];

	function handleDimSum(e: CustomEvent<{ rows: number; cols: number }>) {
		dims = e.detail;
		state = State.VAL;
	}

	function handleValSum(e: CustomEvent<{ values: (string | number)[][] }>) {
		values = e.detail.values;
		state = State.DISP;
	}
</script>

{#if state === State.DIM}
	<Dimensions on:dim_sum={handleDimSum} />
{:else if state === State.VAL}
	<Values {...dims} on:val_sum={handleValSum} />
{:else}
	<Display {values} />
{/if}
