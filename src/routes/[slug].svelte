<script context="module">
	// export const prerender = true; // you can uncomment to prerender as an optimization
	export const hydrate = true;
	import { MY_TWITTER_HANDLE, REPO_URL, SITE_URL } from '$lib/siteConfig';
	import Comments from '../components/Comments.svelte';
	export async function load({ url, params, fetch }) {
		const slug = params.slug;
		let res = null;
		try {
			res = await fetch(`/api/blog/${slug}.json`);
			if (res.status > 400) {
				return {
					status: res.status,
					error: await res.text()
				};
			}

			return {
				props: {
					json: await res.json(),
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

	/** @type {import('$lib/types').ContentItem} */
	export let json; // warning: if you try to destructure content here, make sure to make it reactive, or your page content will not update when your user navigates

	import { onMount } from 'svelte';


	export let src;
  let showPlayer = false;
  let player;

  
	onMount(async () => {
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

<div class="bg-gray-50 dark:bg-gray-900 dark:border-b dark:border-gray-800">
	<article class="mx-auto mb-16 flex w-full h-full max-w-2xl flex-col items-start justify-center ">
		<h1
			class="title mb-8 flex text-3xl font-bold tracking-tight text-black dark:text-white md:text-5xl "
		>
			<img
				src="https://edgeryders.eu/{json.meta.uploaded_logo.url}"
			/>{json.meta.name.toLowerCase()}
		</h1>
		<div
			class="bg w-full  justify-between sm:flex-col sm:items-start md:flex-row md:items-center"
		>
			<p class="min-w-32 pb-3 italic text-sm  text-gray-600 dark:text-gray-400 md:mt-0 ring-yellow-400  " style="-webkit-box-decoration-break: clone;" >
				{json.meta.description_text.substring(0,280)}..
			</p>
		
		</div>
		<div
			class="-mx-4 my-2 flex h-1 w-[100vw] sm:mx-0 sm:w-full"
			style="background: #{json.meta.color}"
		>
			<div class="prose mt-16 mb-32 w-full max-w-none dark:prose-invert" />
		</div>
		<div class="flex">
			<p class="min-w-32 flex items-center text-sm text-gray-600 dark:text-gray-400 md:mt-0 s-YQl-ii7yLz8c"><a href="test" rel="external" class="no-underline s-YQl-ii7yLz8c" target="_blank"><span class="mr-4 font-mono text-xs text-gray-700 text-opacity-70 dark:text-gray-300 s-YQl-ii7yLz8c">updated</span></a> 2022-03-10</p>
			</div>

	</article>
</div>
<div class="mx-auto max-w-7xl w-full text-black dark:text-gray-300">
	<div class="grid mt-12">
		<div class="module-sizer" />
		<div class="module">
			<h1>About {json.meta.name.toLowerCase()}</h1>
			{@html json.meta.description}
		</div>
		<div class="module text-base">
			<h1 class="highlight-container py-2 text-small px-4">
				<span class="highlight">
				{json.topics.pinned[0].title}
			</span>
			</h1>
			<img class="mb-2" src={json.topics.pinned[0].image_url} />
			<p>{@html json.topics.pinned[0].excerpt}</p>
		</div>
		<div class="module module">
			<h1 class="flex items-center">
				<div class="slide_nav ml-2 mr-2">
				<div class="square_nav back"></div>
				<div class="square_nav next"></div>
			</div>
				<div class="text-left ml-1 w-full">Your Submissions</div>
			</h1>
			{#each json.topics.survey.entries as item, i}
				<div class="container-torn">
					<div class="content bg-yellow-50">
					<p class="author dark:text-gray-800 font-bold">{displayEntry(item.properties, 'Name')}</p>
					<span class=" mb-2 dark:text-gray-800 block pb-2 border-b border-black font-mono text-xs text-gray-700 text-opacity-90 dark:text-gray-300"> {displayEntry(item.properties, 'Date')}</span>
					<div class="text-sm  text-gray-600 dark:text-gray-400 leading-tight dark:text-gray-800">{displayEntry(item.properties, 'Text')}</div>
				</div></div>
			{/each}
		</div>
		<div class="module">
			<h1>Videos</h1>
			<div style="display: block; height: 340px">

{#if showPlayer}
{JSON.stringify( json.topics.videos)}


{/if}
</div>
		</div>
		<div class="module">
			<h1>Upcoming Events</h1>
			{#each json.topics.events as item, i}
				<li>
					<a target="_blank" href="https://www.youtube.com/watch?v={item.id}">
						{item.title}
					</a>
				</li>
			{/each}
		</div>
		<div class="module profiles">
			<h1>Who Are We?</h1>
			<div class="square-container">
			{#each json.topics.profiles.yaml as item, i}
				<div class="square" style="background-image: url({item.image})" data-title="{item.title}" data-text="{item.text}">
				</div>
			{/each}
			</div>
		</div>
		<div class="module">
			<h1 class="mt-10">News</h1>
			{#each json.topics.news as item, i}
				<li>
					<a target="_blank" href="https://www.youtube.com/watch?v={item.id}">
						{item.title}
					</a>
				</li>
			{/each}
		</div>
		<div class="module">
			<h1>Scripts</h1>
			{#each json.topics.news as item, i}
				<li>
					<a target="_blank" href="https://www.youtube.com/watch?v={item.id}">
						{item.title}
					</a>
				</li>
			{/each}
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
	.module img {
		height: 170px;
		width:100% ;
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
		@apply border-r border-dashed pr-10 pb-4;
		margin-bottom: 40px;
	}

	.slide_nav {
		@apply flex;
		
	}

	.container-torn  {
  $triangle-size: 4px;
  $stop1: $triangle-size * 1.42;
  $stop2: $triangle-size * 0.7;
  $stop1r: $stop1 + 0.01;
  $stop2r: $stop2 + 0.01;
  background: 
    linear-gradient(135deg, transparent #{$stop1}, rgb(254, 252, 232) #{$stop1r}) top left,
    linear-gradient(45deg, rgb(254, 252, 232) #{$stop2}, transparent #{$stop2r}) top left,
    linear-gradient(135deg, rgb(254, 252, 232) #{$stop2}, transparent #{$stop2r}) bottom left,
    linear-gradient(45deg, transparent #{$stop1}, rgb(254, 252, 232) #{$stop1r}) bottom left;
  background-repeat: repeat-x;
  background-size: ($triangle-size * 2) $triangle-size;
  padding: $triangle-size 0;

}

.container-torn {
  filter: drop-shadow(0 2px 1px rgba(#000, 0.1));
  margin: 0 auto;

  .content {
   @apply bg-yellow-50;
    padding: 30px;
  }

}
	.square_nav {
		background-color: rgb(8, 8, 8);
		width: 27px;
		height: 27px;
		flex-shrink: 0;
		position: relative;
		margin-right: 2px;
		background-size: 22% !important;

		&:hover {
			cursor: pointer;
		}
		&.back {

			background-image: url("data:image/svg+xml,%3Csvg width='203' height='353' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='white' fill-rule='nonzero'%3E%3Cpath d='M51 101h51v50H51zM102 50h50v51h-50zM152 0h51v50h-51zM152 302h51v51h-51zM102 252h50v50h-50zM51 202h51v50H51zM1 151h50v51H1z'/%3E%3C/g%3E%3C/svg%3E");
			background-repeat: no-repeat !important;
			background-position:  48% 53% !important;
		}
		&.next {

			background-image: url("data:image/svg+xml,%3Csvg width='203' height='353' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='white' fill-rule='nonzero'%3E%3Cpath d='M51 101h51v50H51zM102 50h50v51h-50zM152 0h51v50h-51zM152 302h51v51h-51zM102 252h50v50h-50zM51 202h51v50H51zM1 151h50v51H1z'/%3E%3C/g%3E%3C/svg%3E");
			background-repeat: no-repeat !important;
			background-position:  48% 53% !important;
			transform: scaleX(-1);
		}
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
			@apply text-lg font-bold mb-2 border-b border-dashed pb-2;
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
	.highlight {
  position: relative;
  top: -10px;
  font-size: 16px;

}


.highlight:before, 
.highlight:after {
  content: " ";
  display: block;
  height: 190%;
  width: 110%;
  margin-left: -5%;
  margin-top: -01%;
  margin-right: -5%;
  position: absolute;
  transform: skew(-10deg);
}
.highlight:before {
  background: #fafa;
  z-index:-100;
}
 .highlight {
  z-index: 1;
}

.square {
    float:left;
    position: relative;
    width: 23.5%;
	margin: 1%;
    padding-bottom: 23.5%; /* = width for a 1:1 aspect ratio */
	background-repeat:no-repeat !important;
	background-size: cover !important;
  background-position: 50% 50% !important;
  &:nth-child(4n + 1){
	  margin-left: 0
  }
  &:nth-child(4n){
	  margin-right: 0
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
