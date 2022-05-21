<script>
	export let hoverClass = '';
	export let activeClass = '';
	export let extraClass = '';
	export let svg = '';
	export let isActive = false;
</script>

<button
	on:click
	class="{extraClass} {hoverClass} {activeClass} {isActive
		? 'bg-blue-400  hover:bg-blue-500'
		: ''} hover:bg-gray-200 rounded-lg {svg ? 'pl-8' : 'pl-4'} pr-4 py-2 bg-white text-black"
>
	<slot />
</button>
