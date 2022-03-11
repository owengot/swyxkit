<script context="module">
	import { MY_TWITTER_HANDLE, MY_YOUTUBE, REPO_URL, SITE_TITLE } from '$lib/siteConfig';
</script>

<script>
	import '../../tailwind.css';
	import Nav from '../../components/PageNav.svelte';
</script>

<svelte:head>
	<link
		rel="alternate"
		type="application/rss+xml"
		title={'RSS Feed for ' + SITE_TITLE}
		href="/rss.xml"
	/>
</svelte:head>

<div class="fixed w-full">
	<Nav />
</div>
<main class="flex w-full flex-col justify-center">
	<slot />
</main>
