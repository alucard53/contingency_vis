<script lang="ts">
	import Dimensions from '$lib/components/dimensions.svelte';
	import Values from '$lib/components/values.svelte';

	enum State {
		DIM,
		VAL,
		DISP
	}
	let state = State.DIM;

	let dims = { rows: 0, cols: 0 };

	function handleDimSum(e: CustomEvent<{ rows: number; cols: number }>) {
		dims = e.detail;
		state = State.VAL;
	}
</script>

{#if state === State.DIM}
	<Dimensions on:dim_sum={handleDimSum} />
{:else if state === State.VAL}
	<Values {...dims} />
{:else}{/if}
