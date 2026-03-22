<script>
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';

	import { page } from '$app/state';

	let el;

	import {onMount} from 'svelte';

	let { children } = $props();

	function hackClubWindow() {
		window.open("https://hackclub.com/");
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

			let seed;

			if (page.url.pathname === '/') {
				seed = hashSeed("ysws-landing-game-controller");
			} else {
				seed = hashSeed("game-controller");
			}
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

</script>


		<div class="scanlines"></div>
		<div class="flicker"></div>
		<button class="hsCon fixed mt-8 z-50" onclick={hackClubWindow}>
			<img src="/Images/flag-orpheus-left.png" class="w-48 flag" alt="Hackclub Flag">
		</button>

<div class="landing-root">
	<div class="landing-stars-layer" bind:this={el}>

	</div>
</div>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>
{@render children()}

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

    .flag {
        transition: all 0.3s ease;
    }

    .flag:hover {
        opacity: 0.6;
        cursor: pointer;
    }

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