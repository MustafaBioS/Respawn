<script>

	import { onMount } from 'svelte';
	import Lenis from 'lenis';

	onMount(() => {
		const lenis = new Lenis({
			autoRaf: true,
		});
	});

	let started = $state(false)

	function keyPress() {
		started = true
	}

	let isMobile = $state(false)

	onMount(() => {
		isMobile = /iPhone|iPad|Android/i.test(navigator.userAgent)
	})

	function hackClubWindow() {
		window.open("https://hackclub.com/");
	}

	function hackClub() {
		window.location.href = "https://hackclub.com/";
	}

	function RSVP() {
		window.location.href = "https://forms.fillout.com/t/t33bRksw6dus/";
	}

</script>

<svelte:window onclick={keyPress} ontouchstart={keyPress}></svelte:window>

<button class="hsCon fixed mt-8 z-50" onclick={hackClubWindow}>
	<img src="/Images/flag-orpheus-left.png" class="w-48 flag" alt="Hackclub Flag">
</button>

<!--<div class="flex h-full w-full justify-end fixed select-none pointer-events-none">-->
<!--	<div class="rounded-full w-12 h-12 bg-[#E1C418] text-white text-center text-2xl m-10 flex items-center justify-center">?</div>-->
<!--</div>-->

<!--{#if !started}-->

<!--	<div class="startup h-screen w-screen bg-black absolute flex items-center justify-center">-->
<!--		<h1 class="blink text-white text-[clamp(1rem,5vw,3.5rem)] text-center">{isMobile ? '[ TAP TO START ]' : '[ PRESS ANY KEY ]'}</h1>-->
<!--	</div>-->

<!--{:else}-->

	<div class="con flex h-screen w-full items-center justify-center text-center flex-col gap-5">
		<h1 class="title font-bold text-5xl text-[#E1C418]">Respawn</h1>
		<p class="text-white text-l w-[35%] mt-2">Make An Arcade Classic Game With Your Own Twist Added To It, Get Rewards!</p>
		<button title="Button" onclick={RSVP} class="yellow btn w-90 bg-black border-3 border-b-9 h-16 border-[#E1C418] text-white mt-7 cursor-pointer">RSVP</button>
		<button title="Button" class="blue btn w-90 bg-black border-3 border-b-9 mt-3 h-16 border-[#06aecc] text-white cursor-pointer">FAQ</button>
		<button title="Button" onclick={hackClub} class="purple btn w-90 bg-black border-3 mt-3 border-b-9 h-16 border-[#bf5fff] text-white cursor-pointer">Exit</button>
	</div>

	<div class="con flex h-screen w-full text-center items-center flex-col">
		<img src="/Images/car.png" class="car z-50 -mt-24 pt-0 w-60 absolute" alt="car">
		<div class="road mt-0 pt-0"></div>
		<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 place-items-center justify-items-center items-center h-full w-full">
			<div class="h-137.5 w-96 rounded-2xl bg-white"></div>
			<div class="h-137.5 w-96 rounded-2xl bg-white"></div>
			<div class="h-137.5 w-96 rounded-2xl bg-white md:col-span-2 lg:col-span-1 md:mx-auto"></div>
		</div>
	</div>

<!--{/if}-->

<style>

	.car {
		animation: car-move 3s infinite;
	}

	.btn {
      transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

	.btn:hover {
			transform: translateY(-6px);
			box-shadow: 0 2px 25px rgba(83, 31, 168, 0.8);
	}

	@keyframes car-move {
			0% {transform: translateY(0)}
			50% {transform: translateY(10px)}
			75% {transform: translateY(-10px)}
			100% {transform: translateY(0)}
  }

	.road {
			background-color: #2a2a2a;
			height: 65px;
			width: 100%;
			background-image:
				repeating-linear-gradient(
								90deg,
								#F5C200 0px, #F5C200 30px,
								transparent 30px, transparent 60px
			);
			border-top: 4px solid #F5C200;
      border-bottom: 4px solid #F5C200;
	}

    .flag {
			transition: all 0.3s ease;
	}

	.flag:hover {
			opacity: 0.6;
			cursor: pointer;
	}

	.con {
			font-family: "Press Start 2P", system-ui;
	}

  .hs {
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