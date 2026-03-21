<script>
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<div class="scanlines"></div>
<div class="flicker"></div>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>
{@render children()}

<style>


    .scanlines {
        position: fixed;
        inset: 0;
        background: repeating-linear-gradient(
						0deg,
						transparent,
						transparent 2px,
						rgba(0, 0, 0, 0.30) 2px,
						rgba(0, 0, 0, 0.30) 4px
        );
        pointer-events: none;
        z-index: 100;
    }

    .flicker {
        position: fixed;
        inset: 0;
        pointer-events: none;
        z-index: 99;
        background: rgba(0, 0, 0, 0.08);
        animation: flicker 2s infinite;
    }

    @keyframes flicker {
        0%, 90%, 100% { opacity: 1; }
        92% { opacity: 0.6; }
        94% { opacity: 1; }
        96% { opacity: 0.4; }
        98% { opacity: 1; }
    }
</style>