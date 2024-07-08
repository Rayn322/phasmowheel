<script>
	import '../app.css';
</script>

<div class="flex h-svh flex-col">
	<header class="bg-black p-4 text-white">
		<h1 class="text-2xl">Phasmo Wheel</h1>
	</header>

	<main class="flex-grow bg-neutral-200 p-4">
		<slot />
	</main>
</div>
