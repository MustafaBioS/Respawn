<script>

	import { onMount } from 'svelte';


	let started = $state(false)

	function keyPress() {
		started = true
	}

	let isMobile = $state(false)

	onMount(() => {
		isMobile = /iPhone|iPad|Android/i.test(navigator.userAgent)
	})

</script>

<svelte:window onclick={keyPress} ontouchstart={keyPress}></svelte:window>

<div class="con flex h-full w-full items-center justify-center text-center flex-col pt-48">
	<h1 class="title font-bold text-5xl text-[#E1C418]">Respawn</h1>
</div>

{#if !started}

	<div class="startup h-screen w-screen bg-black absolute flex items-center justify-center">
		<h1 class="blink text-white text-[clamp(1rem,5vw,3.5rem)] text-center">{isMobile ? '[ TAP TO START ]' : '[ PRESS ANY KEY ]'}</h1>
	</div>

{:else}

	<div class="con flex h-full w-full items-center justify-center text-center flex-col gap-5 pt-48">
		<p class="text-white text-l w-[35%] mt-2">Make An Arcade Classic Game With Your Own Twist Added To It, Get Rewards!</p>
		<button title="Button" class="w-96 bg-black border-2 border-b-8 h-16 border-[#E1C418] text-white mt-7">Submit</button>
		<button title="Button" class=""></button>
	</div>

{/if}

<style>

	.con {
			font-family: "Press Start 2P", system-ui;
	}

	.title {
      text-shadow:
      -2px -2px 0 #9E5A26,
      2px -2px 0 #9E5A26,
      -2px  5px 0 #9E5A26,
      5px  5px 0 #9E5A26;
	}

	.blink {
      font-family: "Press Start 2P", system-ui;
      font-weight: 400;
      font-style: normal;
			animation: blink 1.5s step-start infinite;
	}

	@keyframes blink {
			0%, 100% { opacity: 1; }
			50% { opacity: 0; }
	}
</style>