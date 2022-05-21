<script>
	import { onMount } from 'svelte';
	import Slider from '../components/slider/Slider.svelte';
	import Play from '../components/controls/Play.svelte';
	import Pause from '../components/controls/Pause.svelte';
	import Stop from '../components/controls/Stop.svelte';

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
				circle.classList.toggle('move');
			} else {
				// check for next iteration
				const now = new Date().getTime();
				const target = Math.floor(60000 / bpm);

				if (now - date >= Math.floor(target)) {
					console.log('is:', now - date, 'should be:', target);
					circle.classList.toggle('move');
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

<div class="p-3 bg-gray-700 text-white rounded-3xl mx-auto shadow-xl w-full md:w-1/2">
	<div class="circle-container bg-zinc-800 rounded-full  mx-auto h-5 block relative w-1/2">
		<div class="circle" bind:this={circle} />
	</div>
	<Slider on:slide={(e) => handleSlide(e.detail)} value={bpm} />
	<div class="flex gap-3 my-3 justify-center items-center">
		<Play {hasStarted} on:play={handlePlay} />
		<Pause on:pause={handlePause} />
		<Stop on:stop={handleStop} />
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
</style>
