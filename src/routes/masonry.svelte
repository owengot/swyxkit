<script context="module">
	// export const prerender = true; // turned off so it refreshes quickly
	export async function load({ params, fetch }) {
		const res = await fetch(`/api/listContent.json`);
		// alternate strategy https://www.davidwparker.com/posts/how-to-make-an-rss-feed-in-sveltekit
		// Object.entries(import.meta.glob('./*.md')).map(async ([path, page]) => {
		if (res.status > 400) {
			return {
				status: res.status,
				error: await res.text()
			};
		}

		/** @type {import('$lib/types').ContentItem[]} */
		const items = await res.json();
		return {
			props: { items },
			maxage: 60 // 1 minute
		};
	}
</script>

<script>
	import { onMount } from 'svelte';

	onMount(async () => {
		var elem = document.querySelector('.grid');
		var msnry = new Masonry(elem, {
			// options
			itemSelector: '.grid-item',
			columnWidth: '.grid-sizer',
			itemSelector: '.grid-item',
			percentPosition: true,
			gutter: 33
		});
	});
</script>

<svelte:head>
	<title>Swyxkit Blog Index</title>
	<meta name="description" content="Latest Hacker News stories in the category" />
</svelte:head>

<section class="w-full">
	<h1>Masonry - columnWidth</h1>

	<div class="grid w-full">
		<div class="grid-sizer" />

		<div class="grid-item">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nunc erat, condimentum eget
			posuere vel, fringilla ut nisi. Suspendisse ipsum lacus, vestibulum vel maximus nec, tincidunt
			et nibh. Proin pharetra, magna ac suscipit tincidunt, quam erat luctus eros, a faucibus est
			magna in elit. Vivamus at metus eleifend, suscipit lacus nec, eleifend dolor. Donec gravida
			augue a massa efficitur, quis vulputate tortor scelerisque. Nullam felis elit, ultrices a
			cursus a, rutrum at leo. Pellentesque nibh lorem, sagittis id tempus eu, volutpat et neque.
			Nullam neque neque, rutrum eget nulla vel, ornare tincidunt nulla. In tincidunt faucibus
			dictum. Mauris at interdum urna. Donec facilisis enim sem. Aliquam semper congue facilisis.
			Mauris tincidunt magna id eros dignissim, ut elementum urna dignissim. Nulla finibus auctor
			mollis. Vivamus varius molestie ipsum, a posuere diam aliquam eu.
		</div>
		<div class="grid-item">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nunc erat, condimentum eget
			posuere vel, fringilla ut nisi. Suspendisse ipsum lacus, vestibulum vel maximus nec, tincidunt
			et nibh. Proin pharetra, magna ac suscipit tincidunt, quam erat luctus eros, a faucibus est
			magna in elit. Vivamus at metus eleifend, suscipit lacus nec, eleifend dolor. Donec gravida
			augue a massa efficitur, quis vulputate tortor scelerisque. Nullam felis elit, ultrices a
			cursus a, rutrum at leo. Pellentesque nibh lorem, sagittis id tempus eu, volutpat et neque.
			Nullam neque neque, rutrum eget nulla vel, ornare tincidunt nulla. In tincidunt faucibus
			dictum. Mauris at interdum urna. Donec facilisis enim sem. Aliquam semper congue facilisis.
			Mauris tincidunt magna id eros dignissim, ut elementum urna dignissim. Nulla finibus auctor
			mollis. Vivamus varius molestie ipsum, a posuere diam aliquam eu.
		</div>
		<div class="grid-item">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nunc erat, condimentum eget
			posuere vel, fringilla ut nisi. Suspendisse ipsum lacus, vestibulum vel maximus nec, tincidunt
			et nibh. Proin pharetra, magna ac suscipit tincidunt, quam erat luctus eros, a faucibus est
			magna in elit. Vivamus at metus eleifend, suscipit lacus nec, eleifend dolor. Donec gravida
			augue a massa efficitur, quis vulputate tortor scelerisque. Nullam felis elit, ultrices a
			cursus a, rutrum at leo. Pellentesque nibh lorem, sagittis id tempus eu, volutpat et neque.
			Nullam neque neque, rutrum eget nulla vel, ornare tincidunt nulla. In tincidunt faucibus
			dictum. Mauris at interdum urna. Donec facilisis enim sem. Aliquam semper congue facilisis.
			Mauris tincidunt magna id eros dignissim, ut elementum urna dignissim. Nulla finibus auctor
			mollis. Vivamus varius molestie ipsum, a posuere diam aliquam eu. Lorem ipsum dolor sit amet,
			consectetur adipiscing elit. Integer nunc erat, condimentum eget posuere vel, fringilla ut
			nisi. Suspendisse ipsum lacus, vestibulum vel maximus nec, tincidunt et nibh. Proin pharetra,
			magna ac suscipit tincidunt, quam erat luctus eros, a faucibus est magna in elit. Vivamus at
			metus eleifend, suscipit lacus nec, eleifend dolor. Donec gravida augue a massa efficitur,
			quis vulputate tortor scelerisque. Nullam felis elit, ultrices a cursus a, rutrum at leo.
			Pellentesque nibh lorem, sagittis id tempus eu, volutpat et neque. Nullam neque neque, rutrum
			eget nulla vel, ornare tincidunt nulla. In tincidunt faucibus dictum. Mauris at interdum urna.
			Donec facilisis enim sem. Aliquam semper congue facilisis. Mauris tincidunt magna id eros
			dignissim, ut elementum urna dignissim. Nulla finibus auctor mollis. Vivamus varius molestie
			ipsum, a posuere diam aliquam eu.
		</div>
		<div class="grid-item grid-item--width2">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nunc erat, condimentum eget
			posuere vel, fringilla ut nisi. Suspendisse ipsum lacus, vestibulum vel maximus nec, tincidunt
			et nibh. Proin pharetra, magna ac suscipit tincidunt, quam erat luctus eros, a faucibus est
			magna in elit. Vivamus at metus eleifend, suscipit lacus nec, eleifend dolor. Donec gravida
			augue a massa efficitur, quis vulputate tortor scelerisque. Nullam felis elit, ultrices a
			cursus a, rutrum at leo. Pellentesque nibh lorem, sagittis id tempus eu, volutpat et neque.
			Nullam neque neque, rutrum eget nulla vel, ornare tincidunt nulla. In tincidunt faucibus
			dictum. Mauris at interdum urna. Donec facilisis enim sem. Aliquam semper congue facilisis.
			Mauris tincidunt magna id eros dignissim, ut elementum urna dignissim. Nulla finibus auctor
			mollis. Vivamus varius molestie ipsum, a posuere diam aliquam eu.
		</div>
		<div class="grid-item">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nunc erat, condimentum eget
			posuere vel, fringilla ut nisi. Suspendisse ipsum lacus, vestibulum vel maximus nec, tincidunt
			et nibh. Proin pharetra, magna ac suscipit tincidunt, quam erat luctus eros, a faucibus est
			magna in elit. Vivamus at metus eleifend, suscipit lacus nec, eleifend dolor. Donec gravida
			augue a massa efficitur, quis vulputate tortor scelerisque. Nullam felis elit, ultrices a
			cursus a, rutrum at leo. Pellentesque nibh lorem, sagittis id tempus eu, volutpat et neque.
			Nullam neque neque, rutrum eget nulla vel, ornare tincidunt nulla. In tincidunt faucibus
			dictum. Mauris at interdum urna. Donec facilisis enim sem. Aliquam semper congue facilisis.
			Mauris tincidunt magna id eros dignissim, ut elementum urna dignissim. Nulla finibus auctor
			mollis. Vivamus varius molestie ipsum, a posuere diam aliquam eu.
		</div>
		<div class="grid-item">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nunc erat, condimentum eget
			posuere vel, fringilla ut nisi. Suspendisse ipsum lacus, vestibulum vel maximus nec, tincidunt
			et nibh. Proin pharetra, magna ac suscipit tincidunt, quam erat luctus eros, a faucibus est
			magna in elit. Vivamus at metus eleifend, suscipit lacus nec, eleifend dolor. Donec gravida
			augue a massa efficitur, quis vulputate tortor scelerisque. Nullam felis elit, ultrices a
			cursus a, rutrum at leo. Pellentesque nibh lorem, sagittis id tempus eu, volutpat et neque.
			Nullam neque neque, rutrum eget nulla vel, ornare tincidunt nulla. In tincidunt faucibus
			dictum. Mauris at interdum urna. Donec facilisis enim sem. Aliquam semper congue facilisis.
			Mauris tincidunt magna id eros dignissim, ut elementum urna dignissim. Nulla finibus auctor
			mollis. Vivamus varius molestie ipsum, a posuere diam aliquam eu.
		</div>
		<div class="grid-item" />
		<div class="grid-item" />
	</div>
</section>

<style lang="scss">
	.grid {
		background: #eee;
		@apply mx-auto;
		width: 85%;
	}

	/* clearfix */
	.grid:after {
		content: '';
		display: block;
		clear: both;
	}

	/* fluid 5 columns */
	.grid-sizer,
	.grid-item {
		width: 31.5%;
		@apply mb-4;
	}
	/* 2 columns wide */
	.grid-item--width2 {
		width: 65.6%;
	}

	/* ---- grid-item ---- */

	.grid-item {
		min-height: 120px;
		float: left;
		background: #d26;
		border: 2px solid #333;
		border-color: hsla(0, 0%, 0%, 0.5);
		border-radius: 5px;
	}
</style>
