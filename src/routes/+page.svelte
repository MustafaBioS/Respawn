<script>

	import { onMount } from 'svelte';
	// import Lenis from 'lenis';

	let el;
	let box;
	let overlay;
	let close;
	let btn;

	// onMount(() => {
	// 	const lenis = new Lenis({
	// 		autoRaf: true,
	// 	});
	// }); // Smooth Scrolling

	let started = $state(false)

	function keyPress() {
		started = true
	}

	let isMobile = $state(false)

	onMount(() => {
		isMobile = /iPhone|iPad|Android/i.test(navigator.userAgent)
	})

	function RSVP() {
		window.location.href = "https://forms.fillout.com/t/t33bRksw6dus/";
	}

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
	})  // Background Controller Icons

		let openFaqIndex = $state(-1);

	function toggleFAQ(index) {
		openFaqIndex = openFaqIndex === index ? -1 : index;
	}



	onMount(() => {
		function handleDocClick(e) {
			if (!box) return;
			if (!box.contains(e.target) && !btn.contains(e.target) || close.contains(e.target)) {
				box.style.display = 'none';
				overlay.style.display = 'none';
			}
		}

		document.addEventListener('click', handleDocClick);
		return () => document.removeEventListener('click', handleDocClick);
	});

	function openFAQ() {
		box.style.display = 'flex';
		overlay.style.display = 'block';
	};

	function shop() {
		// window.location.href = "/shop";
		alert("W.I.P");
	}

</script>

<svelte:window onkeydown={keyPress} ontouchstart={keyPress}></svelte:window>

<div bind:this={overlay} class="overlay hidden h-screen w-full bg-black opacity-70 fixed z-99"></div>

<div class="absolute flex items-center justify-center h-screen w-full">
	<div bind:this={box} class="faq hidden flex-col h-165 w-165 rounded-xl bg-[#063e59] border-[#073145] border-5 z-100 overflow-y-auto">

		<div class="top flex flex-row items-start justify-between w-full">
			<h1 class="text-4xl m-10 text-white">Frequently Asked Questions</h1>
			<button bind:this={close} class="close text-4xl m-10 text-white">X</button>
		</div>

		<div class="questions flex flex-col items-start justify-start w-full h-full">

			<button class='q' class:open={openFaqIndex === 0} onclick={() => toggleFAQ(0)}>
				<div class='q-header'>
					What is Hack Club?
					<span class='arrow'>▼</span>
				</div>
				<span class='hiddenDesc'>Hack Club is the world's biggest community of teenage makers! We're a 501(c)(3) nonprofit, supported by folks like GitHub and donors who believe in young builders. We're fully transparent and here to help you ship.</span>
			</button>

			<button class='q' class:open={openFaqIndex === 1} onclick={() => toggleFAQ(1)}>
				<div class='q-header'>
					Who can submit?
					<span class='arrow'>▼</span>
				</div>
				<span class='hiddenDesc'>Respawn is open to all Hack Clubbers. All teenagers 18 and under, anywhere in the world can participate.</span>
			</button>


			<button class='q' class:open={openFaqIndex === 2} onclick={() => toggleFAQ(2)}>
				<div class='q-header'>
					Can i use AI to help make my project?
					<span class='arrow'>▼</span>
				</div>
				<span class='hiddenDesc'>Only up to <span class="font-extrabold">30%</span> of your project may be AI.</span>
			</button>


			<button class='q' class:open={openFaqIndex === 3} onclick={() => toggleFAQ(3)}>
				<div class='q-header'>
					Can I submit Respawn projects to any other YSWS?
					<span class='arrow'>▼</span>
				</div>
				<span class='hiddenDesc'>No, "Double Dipping" is not allowed, if you submit a project to Respawn then you can not submit it to other Hack Club program.</span>
			</button>

			<button class='q' class:open={openFaqIndex === 4} onclick={() => toggleFAQ(4)}>
				<div class='q-header'>
					Do I need to have any prior experience?
					<span class='arrow'>▼</span>
				</div>
				<span class='hiddenDesc'>Nope! Respawn is completely beginner friendly and we welcome people with any amount of experience to join and make cool projects!</span>
			</button>

		</div>

	</div>
</div>

{#if !started}

	<div class="startup h-screen w-screen bg-black absolute flex items-center justify-center">
		<h1 class="blink text-white text-[clamp(1rem,5vw,3.5rem)] text-center">{isMobile ? '[ TAP TO START ]' : '[ PRESS ANY KEY ]'}</h1>
	</div>

{:else}


<div class="con flex h-screen w-full items-center justify-center text-center flex-col gap-5">
	<h1 class="title font-bold text-5xl text-[#E1C418]">Respawn</h1>
	<p class="text-white min-w-85 w-[75%] max-w-175 mt-2 txt">Make An Arcade Classic Game With Your Own Twist Added To It, Get Rewards!</p>
	<button title="Button" onclick={RSVP} class="yellow btn w-90 bg-black border-3 border-b-9 h-16 border-[#E1C418] text-white mt-7 cursor-pointer">RSVP</button>
	<button title="Button" bind:this={btn} onclick={openFAQ} class="blue btn w-90 bg-black border-3 border-b-9 mt-1 h-16 border-[#06aecc] text-white cursor-pointer">FAQ</button>
	<button title="Button" onclick={shop} class="purple btn w-90 bg-black border-3 mt-1 border-b-9 h-16 border-[#bf5fff] text-white cursor-pointer">Shop</button>
</div>

<!--<div class="con flex h-screen w-full text-center items-center flex-col">-->

<!--	<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 place-items-center justify-items-center items-center h-full w-full">-->

<!--		<div class="gi h-137.5 w-96 rounded-2xl bg-white flex flex-col items-center z-60">-->
<!--			<h1 class="text-3xl mt-17">Build</h1>-->
<!--			<div class="flex h-[60%] w-full items-center justify-center">-->
<!--				<p class="w-[75%]">Build an arcade classic game with your own twist added to it</p>-->
<!--			</div>-->
<!--		</div>-->
<!--		<div class="gi h-137.5 w-96 rounded-2xl bg-white flex flex-col items-cener z-60">-->
<!--			<h1 class="text-3xl mt-17">Ship</h1>-->
<!--			<div class="flex h-[60%] w-full items-center justify-center">-->
<!--				<p class="w-[75%]">Ship your game to something like <a href="http://itch.io" target="_blank">itch.io</a> and your code must be open source</p>-->
<!--			</div>-->
<!--		</div>-->
<!--		<div class="gi h-137.5 w-96 rounded-2xl bg-white flex flex-col items-center md:col-span-2 lg:col-span-1 md:mx-auto z-50">-->
<!--			<h1 class="text-3xl mt-17">Get</h1>-->
<!--			<div class="flex h-[60%] w-full items-center justify-center">-->
<!--				<p class="w-[75%]">Get tokens which you can spend in the shop to buy a lot of cool items like console grants!</p>-->
<!--			</div>-->
<!--		</div>-->
<!--	</div>-->
<!--</div>-->

{/if}

<style>

	.btn {
				width: clamp(17.5rem, 50vw, 22rem);
			}

	.title {
			font-size: clamp(2rem, 5vw, 3rem);
	}

	.txt {
			font-size: clamp(0.75rem, 2.5vw, 1rem);
	}

	.close {
		transition: all 0.3s ease;
	}

	.close:hover {
		opacity: 0.6;
		cursor: pointer;
	}

	.faq {
		font-family: "Phantom Sans";
		font-weight: 400;
		font-style: normal;
	}

  .q {
		width: 100%;
		text-align: left;
		padding: 20px 50px;
		background-color: transparent;
		color: white;
		position: relative;
		cursor: pointer;
	  transition: background-color 0.3s ease, opacity 0.3s ease;
  }

  .q::after {
		content: "";
		position: absolute;
		bottom: 0;
		left: 50%;
		transform: translateX(-50%);
		width: 85%;
		height: 2px;
		background-color: rgb(255, 255, 255);
		transition: all 0.3s ease;
  }

  .q-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 22.5px;
		width: 100%;
	  transition: color 0.3s ease;
  }

  .hiddenDesc {
		font-size: 17px;
		opacity: 0;
	  max-height: 0;
	  overflow: hidden;
		display: block;
	  transition: all 0.3s ease;
  }

	.arrow {
		transition: all 0.3s ease;
	}

	.q.open .hiddenDesc {
		max-height: 10rem;
		opacity: 0.65;
		padding-top: 10px;
	}

	.q.open .arrow {
		transform: rotate(180deg);
	}

	.gi {
			box-shadow: 0 2px 35px rgba(83, 31, 168, 0.8);
			transition: all 0.3s ease;
	}

	.gi:hover {
			transform: translateY(-6px);
	}

	.btn {
      transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

	.btn:hover {
			transform: translateY(-6px);
	}

	.con {
			position: relative;
			z-index: 2;
			font-family: "Press Start 2P", system-ui;
	}

	.yellow:hover {
      box-shadow: 0 2px 25px rgba(225, 196, 24, 0.8);
	}

	.blue:hover {
			box-shadow: 0 2px 25px rgba(6, 174, 204, 0.8);
	}

	.purple:hover {
      box-shadow: 0 2px 25px rgba(191, 95, 255, 0.8);
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