<script>
	import Metronome from '../metronome/Metronome.svelte';
</script>

<div class="page-container h-screen relative">
	<div
		class="container absolute top-1/2 left-1/2 w-full transform -translate-x-1/2 -translate-y-1/2 text-center"
	>
		<Metronome />
		<div class="info mt-8">
			<a
				target="_blank"
				href="https://svelte.dev"
				class="text-xs block text-gray-300 hover:text-yellow-500 tracking-tighter"
				>Made with Svelte</a
			>
			<a
				target="_blank"
				href="https://www.alextana.co.uk"
				class="text-xs block text-gray-300 hover:text-yellow-500 tracking-tighter">alextana.co.uk</a
			>
		</div>
	</div>
</div>
