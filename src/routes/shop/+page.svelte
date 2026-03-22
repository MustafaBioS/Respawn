<script>
	import { onMount } from 'svelte';
	let el;

</script>

<div>
	<div class="landing-root">
		<div class="landing-stars-layer" bind:this={el}>
			test
		</div>
	</div>
</div>

<style>

	.landing-root {
			position: relative;
			isolation: isolate;
	}

	:global(.landing-stars-layer) {
			position: absolute;
			inset: 0;
			pointer-events: none;
			z-index: -2;
	}

	:global(.landing-star) {
			position: absolute;
			pointer-events: none;
	}

	:global(.landing-star-img) {
			display: block;
			opacity: 0.3;
	}

</style>
