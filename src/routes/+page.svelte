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

	let el;

	onMount(() => {
		var iconBase = "https://icons.hackclub.com/api/icons/0xbf5fff";

		function hashSeed(str) {
			var h = 0;
			for (var i = 0; i < str.length; i++) h = ((h << 5) - h) + str.charCodeAt(i) | 0;
			return Math.abs(h);
		}

		function renderIcons() {
			if (!el) return;
			el.innerHTML = "";

			var seed = hashSeed("landing-game-controller");
			function seededRandom() {
				seed = (seed * 9301 + 49297) % 233280;
				return seed / 233280;
			}

			var pageHeight = document.documentElement.scrollHeight;
			var pageWidth = document.documentElement.clientWidth;
			var firstSectionHeight = window.innerHeight;
			var centerMinX = pageWidth * 0.22;
			var centerMaxX = pageWidth * 0.78;
			var centerMinY = firstSectionHeight * 0.2;
			var centerMaxY = firstSectionHeight * 0.85;
			var iconsPerViewport = 10;
			var viewportCount = Math.max(1, Math.ceil(pageHeight / window.innerHeight));
			var n = iconsPerViewport * viewportCount;

			function isInHeroCenterZone(x, y, size) {
				var centerX = x + size / 2;
				var centerY = y + size / 2;
				return centerY <= firstSectionHeight &&
					centerX >= centerMinX &&
					centerX <= centerMaxX &&
					centerY >= centerMinY &&
					centerY <= centerMaxY;
			}

			for (var i = 0; i < n; i++) {
				var size = 34 + Math.floor(seededRandom() * 22);
				var rot = Math.floor(seededRandom() * 360);
				var x = 0;
				var y = 0;
				var maxX = Math.max(0, pageWidth - size);
				var maxY = Math.max(0, pageHeight - size);

				var attempts = 0;
				for (attempts = 0; attempts < 30; attempts++) {
					x = seededRandom() * maxX;
					y = seededRandom() * maxY;
					if (!isInHeroCenterZone(x, y, size)) break;
				}

				if (attempts === 30) {
					if (pageHeight > firstSectionHeight + size) {
						y = firstSectionHeight + seededRandom() * Math.max(0, pageHeight - firstSectionHeight - size);
						x = seededRandom() * maxX;
					} else {
						var leftMax = Math.max(0, centerMinX - size);
						var rightMin = Math.min(maxX, centerMaxX);
						y = seededRandom() * Math.max(0, firstSectionHeight - size);
						if (seededRandom() < 0.5 && leftMax > 0) {
							x = seededRandom() * leftMax;
						} else {
							x = rightMin + seededRandom() * Math.max(0, maxX - rightMin);
						}
					}
				}

				var img = document.createElement("img");
				img.src = iconBase + "/game-controller";
				img.alt = "";
				img.className = "landing-star-img";
				img.setAttribute("width", size);
				img.setAttribute("height", size);
				var wrap = document.createElement("div");
				wrap.className = "landing-star";
				wrap.style.left = x + "px";
				wrap.style.top = y + "px";
				wrap.style.width = size + "px";
				wrap.style.height = size + "px";
				wrap.style.transform = "rotate(" + rot + "deg)";
				wrap.appendChild(img);
				el.appendChild(wrap);
			}
		}

		renderIcons();
		window.addEventListener('resize', renderIcons);

		return () => {
			window.removeEventListener('resize', renderIcons);
			if (el) el.innerHTML = "";
		};
	});

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

<div class="landing-root">
	<div class="landing-stars-layer" bind:this={el}></div>
		<div class="con flex h-screen w-full items-center justify-center text-center flex-col gap-5">
			<h1 class="title font-bold text-5xl text-[#E1C418]">Respawn</h1>
			<p class="text-white text-l w-[35%] mt-2">Make An Arcade Classic Game With Your Own Twist Added To It, Get Rewards!</p>
			<button title="Button" onclick={RSVP} class="yellow btn w-90 bg-black border-3 border-b-9 h-16 border-[#E1C418] text-white mt-7 cursor-pointer">RSVP</button>
			<button title="Button" class="blue btn w-90 bg-black border-3 border-b-9 mt-3 h-16 border-[#06aecc] text-white cursor-pointer">FAQ</button>
			<button title="Button" onclick={hackClub} class="purple btn w-90 bg-black border-3 mt-3 border-b-9 h-16 border-[#bf5fff] text-white cursor-pointer">Exit</button>
		</div>

		<div class="con flex h-screen w-full text-center items-center flex-col">
			<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 place-items-center justify-items-center items-center h-full w-full">
				<div class="gi h-137.5 w-96 rounded-2xl bg-white flex flex-col items-center z-100">
					<h1 class="text-3xl mt-17">Build</h1>
					<div class="flex h-[60%] w-full items-center justify-center">
						<p class="w-[75%]">Build an arcade classic game with your own twist added to it</p>
					</div>
				</div>

				<div class="gi h-137.5 w-96 rounded-2xl bg-white flex flex-col items-center">
					<h1 class="text-3xl mt-17">Ship</h1>
					<div class="flex h-[60%] w-full items-center justify-center">
						<p class="w-[75%]">Build an arcade classic game with your own twist added to it</p>
					</div>
				</div>
				<div class="gi h-137.5 w-96 rounded-2xl bg-white flex flex-col items-center md:col-span-2 lg:col-span-1 md:mx-auto">
					<h1 class="text-3xl mt-17">Get</h1>
					<div class="flex h-[60%] w-full items-center justify-center">
						<p class="w-[75%]">Build an arcade classic game with your own twist added to it</p>
					</div>
				</div>
			</div>
		</div>
</div>

<!--{/if}-->

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

	.gi {
			box-shadow: 0 2px 35px rgba(83, 31, 168, 0.8);
			transition: all 0.3s ease;
	}

	.gi:hover {
			transform: translateY(-6px);
	}

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
			background-color: white;
			height: 65px;
			width: 100%;
			background-image:
				repeating-linear-gradient(
								90deg,
								#F5C200 10px, #32fffe 20px, #2CFE16 30px, #ED00FC 40px, #DF0000 50px, #0000E3 60px,
								transparent 30px, transparent 60px
			);
	}

    .flag {
			transition: all 0.3s ease;
	}

	.flag:hover {
			opacity: 0.6;
			cursor: pointer;
	}

	.con {
			position: relative;
			z-index: 2;
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