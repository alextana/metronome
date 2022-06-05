<script>
	import '../app.css';
</script>

<slot />

<style>
	:global(body) {
		background: rgb(85, 81, 81);
		background: radial-gradient(circle, rgba(85, 81, 81, 1) 0%, rgba(36, 36, 36, 1) 75%);
	}
</style>
