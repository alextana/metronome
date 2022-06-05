<script>
	import { onMount } from 'svelte';
	import Slider from '../components/slider/Slider.svelte';
	import Play from '../components/controls/Play.svelte';
	import Pause from '../components/controls/Pause.svelte';
	import { fly } from 'svelte/transition';

	let bpm = 120;
	let audio = null;
	let circle = null;

	let date = new Date().getTime();
	let hasStarted = false;
	let playInterval = null;

	onMount(() => {
		audio = new Audio('click.wav');
	});

	function handleStop() {
		if (!playInterval) {
			return;
		}

		hasStarted = false;

		clearInterval(playInterval);
		bpm = 120;
	}

	function handlePlay() {
		if (hasStarted) {
			return;
		}
		playInterval = setInterval(() => {
			// first iteration
			if (!hasStarted) {
				hasStarted = true;
				date = new Date().getTime();
				audio.play();
				circle?.classList.toggle('move');
			} else {
				// check for next iteration
				const now = new Date().getTime();
				const target = Math.floor(60000 / bpm);

				if (now - date >= Math.floor(target)) {
					// console.log('is:', now - date, 'should be:', target);
					circle?.classList.toggle('move');
					date = new Date().getTime();
					// clone node so it can be played
					// before the previous iteration has finished
					audio.cloneNode().play();
				}
			}
		}, 1);
	}

	function handlePause() {
		if (!playInterval) {
			return;
		}

		hasStarted = false;

		clearInterval(playInterval);
	}

	function handleSlide(e) {
		bpm = e.value;

		if (playInterval) {
			handlePause();
			handlePlay();
		}
	}

	function handleBlur() {
		if (playInterval) {
			handlePause();
		}
	}
</script>

<div
	class="p-6 metronome relative border-8 border-black/30 bg-neutral-800 text-white rounded-3xl mx-auto w-full md:w-1/2"
>
	{#if hasStarted}
		<div
			transition:fly={{ y: -10, duration: 100 }}
			class="circle-container absolute top-6 left-1/2 transform -translate-x-1/2 bg-zinc-900/30 rounded-full  mx-auto h-5 block w-1/2 mb-4"
		>
			<div class="circle" bind:this={circle} />
		</div>
	{/if}
	<div
		class="mb-4 mt-4 bg-black/30 absolute top-1/2 left-1/2 transform -translate-y-1/2 -translate-x-1/2 w-full p-6"
	>
		<Slider on:slide={(e) => handleSlide(e.detail)} value={bpm} />
	</div>
	<div class="flex absolute bottom-6 left-0 p-6 w-full gap-3 my-3 justify-center items-center">
		{#if hasStarted}
			<Pause on:pause={handlePause} />
		{:else}
			<Play {hasStarted} on:play={handlePlay} />
		{/if}
	</div>
</div>

<svelte:window on:blur={handleBlur} />

<style>
	.circle {
		width: 20px;
		height: 20px;
		border-radius: 50%;
		background: #fff;
		position: absolute;
		transition: 0.3s all cubic-bezier(0.85, 0, 0.15, 1);
		transform: translateX(0);
		left: 0px;
	}
	:global(.circle.move) {
		left: calc(100% - 20px);
	}

	.metronome {
		width: 350px;
		height: 70vh;
		max-height: 600px;
		border-radius: 50px;
		background: #383838;
		box-shadow: 20px 20px 60px #303030, -20px -20px 60px #404040;
	}
</style>
