<script>
	import { createEventDispatcher } from 'svelte';
	export let value = 0;

	const dispatch = createEventDispatcher();

	function handleSlide() {
		dispatch('slide', { value });
	}
</script>

<p class="text-6xl font-extrabold tracking-tighter italic">{value}</p>
<div class="slider-container">
	<input
		class="w-full h-6 p-0 bg-white focus:outline-none focus:ring-0 focus:shadow-none"
		type="range"
		min="1"
		max="300"
		bind:value
		on:input={handleSlide}
	/>
</div>
