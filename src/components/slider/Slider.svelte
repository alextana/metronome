<script>
	import { createEventDispatcher } from 'svelte';
	export let value = 0;

	const dispatch = createEventDispatcher();

	function handleSlide() {
		dispatch('slide', { value });
	}
</script>

<p class="text-6xl font-extrabold tracking-tighter italic">
	{value} <span class="text-4xl">BPM</span>
</p>
<div class="slider-container mt-4">
	<input
		class="form-range
		appearance-none w-full h-3 rounded-full p-0 bg-zinc-900 focus:outline-none focus:ring-0 focus:shadow-none"
		type="range"
		min="1"
		max="300"
		bind:value
		on:input={handleSlide}
	/>
</div>

<style>
	input[type='range']::-webkit-slider-thumb {
		appearance: none;
		width: 26px;
		height: 26px;
		border-radius: 50%;
		background: rgb(234 179 8) !important;
	}
</style>
