<script context="module">
	// export const prerender = true; // you can uncomment to prerender as an optimization
	export const hydrate = true;
	import { MY_TWITTER_HANDLE, REPO_URL, SITE_URL } from '$lib/siteConfig';
	export async function load({ url, params, fetch }) {
		const slug = params.slug;
		let res = null;
		let top = null;
		try {
			res = await fetch(`/api/blog/treasure.json`);
			top = await fetch(`/api/page/${slug}.json`);
			if (res.status > 400) {
				return {
					status: res.status,
					error: await res.text()
				};
			}

			if (top.status > 400) {
				return {
					status: top.status,
					error: await top.text()
				};
			}

			return {
				props: {
					json: await res.json(),
					topic: await top.json(),
					slug,
					REPO_URL
				},
				maxage: 60 // 1 minute
			};
		} catch (err) {
			console.error('error fetching blog post at [slug].svelte: ' + slug, res, err);
			return {
				status: 500,
				error: new Error('error fetching blog post at [slug].svelte: ' + slug + ': ' + res)
			};
		}
	}
</script>

<script>
	import 'prism-themes/themes/prism-shades-of-purple.min.css';
	import tippy, {followCursor} from 'tippy.js';
import 'tippy.js/dist/tippy.css'; // optional for styling
import Newsletter from '../../components/Newsletter.svelte';

	/** @type {import('$lib/types').ContentItem} */
	export let json; // warning: if you try to destructure content here, make sure to make it reactive, or your page content will not update when your user navigates
	export let topic;
	import { onMount } from 'svelte';

	onMount(async () => {
		var elem = document.querySelector('.grid');
		var msnry = new Masonry(elem, {
			// options
			itemSelector: '.module',
			columnWidth: '.module-sizer',
			percentPosition: true,
			gutter: 33.33
		});
		tippy('.square', {
			content(reference) {
				const title = reference.getAttribute('data-title');
				const text = reference.getAttribute('data-text');
				return "<h3 class='font-bold border-b pb-3 mb-2'>" + title + "</h3><p>" + text + "</p>"
			},
			allowHTML: true,
			followCursor: true,
  plugins: [followCursor],
      });
	});

	function createDate(string) {
	const nth = function(d) {
		const dString = String(d);
		const last = +dString.slice(-2);
		if (last > 3 && last < 21) return 'th';
		switch (last % 10) {
			case 1:  return "st";
			case 2:  return "nd";
			case 3:  return "rd";
			default: return "th";
		}
	}

	var date = new Date(string);
	var day = date.getDay();
	var year = date.getFullYear();
	var day_long = new Intl.DateTimeFormat('en-US', { weekday: 'long'}).format(date);
	var month = new Intl.DateTimeFormat('en-US', { month: 'long'}).format(date);
	var obj = {
		day_long: day_long,
		month: month,
		day: day + nth(day),
		year: year
	}
	return obj
}

	function displayEntry(item, key){
		if (key == 'Name') {
			return item[key].title[0].text.content
		}
		if (key == 'Title') {
			return item[key].rich_text[0].plain_text
		}
		if (key == 'Text') {
			return item["Share your thoughts"].rich_text[0].plain_text
		}
		if (key == 'Date') {
			return createDate(item[key].created_time).day_long + ' ' + createDate(item[key].created_time).month + ' ' + createDate(item[key].created_time).day + ' ' + createDate(item[key].created_time).year
		}
	
	}
</script>

<svelte:head>
	<!-- <title>{json.title}</title>
	<meta name="description" content="swyxkit blog" />

	<link rel="canonical" href={SITE_URL} />
	<meta property="og:url" content={SITE_URL} />
	<meta property="og:type" content="article" />
	<meta property="og:title" content={json.title} />
	<meta name="Description" content={json.description} />
	<meta property="og:description" content={json.description} />
	<meta name="twitter:card" content={json.image ? 'summary_large_image' : 'summary'} />
	<meta name="twitter:creator" content={'@' + MY_TWITTER_HANDLE} />
	<meta name="twitter:title" content={json.title} />
	<meta name="twitter:description" content={json.description} />
	{#if json.image}
		<meta property="og:image" content={json.image} />
		<meta name="twitter:image" content={json.image} />
	{/if} -->
</svelte:head>

<div class="bg-gray-50 dark:bg-gray-900 dark:border-b dark:border-gray-800 pt-28" style="background: url({json.topics.pinned[0].image_url}); background-position: 0 0; background-size: 150%">
	<header class="mx-auto mb-16 flex w-full h-full max-w-2xl flex-col items-start justify-center ">
		<h1
			class="title mb-8 flex text-3xl font-bold tracking-tight text-black dark:text-white md:text-5xl "
		>
			Tell Us Your Automotive Nightmares!
		</h1>
		<div
			class="bg mt-2 flex w-full justify-between sm:flex-col sm:items-start md:flex-row md:items-center"
		>
			<p class="flex items-center text-sm text-gray-700 dark:text-gray-300">swyx</p>
			<p class="min-w-32 flex items-center text-sm text-gray-600 dark:text-gray-400 md:mt-0">
				<a href="test" rel="external" class="no-underline" target="_blank">
					<span class="mr-4 font-mono text-xs text-gray-700 text-opacity-70 dark:text-gray-300">
						updated</span
					>
				</a>
				{new Date().toISOString().slice(0, 10)}
			</p>
		</div>
		<div
			class="-mx-4 my-2 flex h-1 w-[100vw] sm:mx-0 sm:w-full"
			style="background: #{json.meta.color}"
		>
			<div class="prose mt-16 mb-32 w-full max-w-none dark:prose-invert" />
		</div>
	</header>
</div>
<div class="mx-auto max-w-2xl w-full text-black dark:text-gray-300">
	<div class="w-full mt-10">
		
	
		{#each topic.content as item, i}
			<div class="mb-4">
				{#if item.type == 'title'}
					<h2 class="font-bold text-2xl mt-10 border-b pb-3 mb-3">{@html item.content}</h2>
				{/if}
				{#if item.type == 'text'}
					{@html item.content}
				{/if}
				{#if item.type == 'form'}
					<div class="mt-4 border rounded-lg p-8" style="background: rgba(254, 249, 195, 0.2)">
						{#each Object.entries(item.entries).reverse() as [key, value]}
							{#if value.type == 'title'}
							<div class="mb-2">
								<h3 class="font-bold text-lg">{key}</h3>
								<input class="border p-2 px4 w-full" />
							</div>
							{/if}
							{#if value.type == 'email'}
							<div class="mb-2">
								<h3 class="font-bold text-lg">{key}</h3>
								<input type="email" class="border p-2 px4 w-full" placeholder="email" />
							</div>
							{/if}
							{#if value.type == 'rich_text'}
							<div class="mb-2">
								<h3 class="font-bold text-lg">{key}</h3>
								<textarea placeholder="enter text" class="border h-40 p-3 mt-4 w-full" />
							</div>
							{/if}
							{#if value.type == 'checkbox'}
							<div class="my-3">
								<h3 class="font-bold mb-1 text-lg border-b pb-2 mb-3">{key}</h3>
								<div class="pt-1">
									<input type="radio"/>
									<label>Yes</label>
									<input type="radio"/>
									<label>No</label>
								</div>
							</div>
							{/if}
					{/each}	
					<div class="my-3">
						<h3 class="font-bold mb-1 text-lg border-b pb-2 mb-3">GDPR Agreement</h3>
						<p> Edgeryders will not use your information for third party services, but may post your submission anonymously to our discussion platform for research purposes.</p>
						<div class="pt-2">
							<input class="mr-1" type="checkbox"/>
							<label class="font-bold">I consent and have read the Edgeryders terms and conditions.</label>
						</div>
					</div>
					<button type="submit" class="rounded bg-gray-800 py-3 text-white mt-4 px-6">Submit</button>
					</div>
				{/if}
				{#if item.type == 'newsletter'}
					<Newsletter />
				{/if}
			</div>
		{/each}

		<div class="w-full profiles">
			<h1>Who Are We?</h1>
			<div class="square-container">
			{#each json.topics.profiles.yaml as item, i}
				<div class="square" style="background-image: url({item.image})" data-title="{item.title}" data-text="{item.text}">
				</div>
			{/each}
		</div>
		</div>
	</div>
</div>

<style lang="scss">
	.grid {
		width: 100%;
	}

	/* clearfix */
	.grid:after {
		content: '';
		display: block;
		clear: both;
	}

	/* fluid 5 columns */
	.module-sizer,
	.module {
		width: 31.5%;
	}
	/* 2 columns wide */
	.module-w2 {
		width: 66%;clear: both;
	}

	.module-h2 {
		height: 800px;
		
	}

	/* ---- grid-item ---- */

	.module {
		min-height: 20px;
		float: left;
		@apply pt-4;
		margin-bottom: 30px;
	}

	h1 {
		text-transform: capitalize;
	}
	h1 img {
		height: 60px;
		@apply mr-5;
		background: none;
	}
	.module {
		@apply mb-2;

		h1 {
			@apply text-xl font-bold mb-2 border-b pb-2;
		}
		:global(p) {
			font-size: 16px;
		}
	}
	.module.profiles {
		li {
			width: 20%;
			height: 100px;
			float: left;
			list-style-type: none;
			img {
				width: 100%;
			}
		}
	}


.square {
    float:left;
    position: relative;
    width: 13.3%;
	margin: .53%;
    padding-bottom: 13.3%; /* = width for a 1:1 aspect ratio */
	background-repeat:no-repeat !important;
	background-size: cover !important;
  background-position: 50% 50% !important;
  &:nth-child(6n + 1){
	  margin-left: 0
  }
  &:hover {
	  cursor: pointer;
	  &:after {
		  position: absolute;
		  width: 100%;
		  height: 100%;
		  content: '';
		  background: rgba(0,0,0,0.8);
	  }
  }
}



</style>
