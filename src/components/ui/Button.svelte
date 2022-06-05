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
		? ' block hover:bg-blue-500'
		: ''} hover:bg-gray-200 rounded-lg {svg
		? 'pl-8'
		: 'pl-4'} w-full text-center flex justify-center pr-4 py-2 bg-yellow-500 hover:bg-yellow-600 text-black"
>
	<slot />
</button>
