<script>
	import MobileMenu from './MobileMenu.svelte';
	import { REPO_URL } from '$lib/siteConfig';
	import NavLink from './NavLink.svelte';
	let isDark = false;
	if (typeof localStorage !== 'undefined') {
		if (
			localStorage.theme === 'dark' ||
			(!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)
		) {
			isDark = true;
		}
	}
	function toggleDarkMode() {
		if (isDark) {
			document.documentElement.classList.remove('dark');
			localStorage.theme = 'light';
			isDark = false;
		} else {
			document.documentElement.classList.add('dark');
			localStorage.theme = 'dark';
			isDark = true;
		}
	}
</script>

<nav
	class="relative mx-auto flex w-full items-center justify-between border-gray-200
	py-3 pt-8 text-gray-900 dark:border-gray-700 dark:bg-gray-900
	dark:text-gray-100"
>
<div class="max-w-2xl flex w-full items-center justify-between mx-auto">
	<a href="#skip" class="skip-nav">Skip to content</a>
	<MobileMenu />
	<ul class="ml-[-0.60rem] flex">
		<svg style="width: 55px; padding-right: 15px; margin: 0 7px;" class="border-r border-gray-300" viewBox="0 0 500 500" xmlns="http://www.w3.org/2000/svg"><path d="M0 250a250 250 0 1 1 500 0 250 250 0 0 1-500 0zm97-103v106h42v-16h-26v-31h19v-16h-19v-27h26v-16zm80 91c6 0 8-4 8-11v-52c0-8-4-12-13-12h-4v75zm-25-91h23c19 0 27 10 27 29v48c0 20-8 29-26 29h-24zm63 25c0-17 9-27 25-27s24 9 24 27v10h-16v-11c0-8-3-11-8-11-6 0-9 3-9 11v59c0 7 3 11 9 11 5 0 8-4 8-11v-17h-10v-13h26v53h-8l-3-7c-4 7-9 10-16 10-14 0-22-11-22-30zm64-25h43v16h-26v27h18v16h-18v31h26v16h-43zM112 381H95V275h24c18 0 26 8 26 30 0 13-3 21-10 26l12 50h-17l-10-45h-8zm0-60v-31h6c8 0 10 3 10 15 0 11-2 16-10 16zm53-46l10 42h1l10-42h17l-19 62v44h-16v-42l-20-64zm69 90c6 0 9-4 9-11v-51c0-9-4-13-13-13h-4v75zm-24-90h23c18 0 27 9 27 29v47c0 20-8 30-26 30h-24zm136 46v-31h6c8 0 10 3 10 15 0 11-2 16-10 16zm-72-46h43v15h-26v28h19v15h-19v32h26v16h-43zm72 106h-16V275h24c17 0 25 8 25 30 0 13-3 21-10 26l12 50h-17l-10-45h-8zm57-30c1 11 4 16 11 16 6 0 10-4 10-10 0-9-6-14-15-23-12-12-20-21-20-35 0-15 11-26 25-26s24 10 25 26l-16 2c-1-9-3-13-9-13-5 0-9 4-9 10 0 9 7 16 17 25 11 11 18 19 18 34s-11 26-26 26-26-11-27-29zm-65-172v-42h-42a42 42 0 1 1 42 42z" fill="orangered" fill-rule="evenodd"></path></svg>
		<img style="background: none; width:auto; height: 40px; margin: 0 10px" src="https://edgeryders.eu//uploads/default/original/2X/2/2fe45ff027f9b11c5532e15407d838d3eee882de.png" >
		<li>
			<NavLink href="/">Home</NavLink>
		</li>
		<li>
			<NavLink href="/blog">Blog</NavLink>
		</li>
		<li>
			<NavLink href="/about">About</NavLink>
		</li>
	</ul>
	<div class="flex items-center space-x-4">
		<!-- RSS -->
		<!-- <a
			class="rounded-lg text-gray-700 hover:bg-yellow-200 dark:text-gray-200 dark:hover:bg-yellow-800"
			rel="external"
			href={origin + '/api/rss.xml'}
			aria-label="RSS"
		>
			<svg
				aria-hidden="true"
				xmlns="http://www.w3.org/2000/svg"
				class="h-9 w-9 p-1"
				fill="currentColor"
				viewBox="0 0 24 24"
				stroke="currentColor"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M6 5c7.18 0 13 5.82 13 13M6 11a7 7 0 017 7m-6 0a1 1 0 11-2 0 1 1 0 012 0z"
				/>
			</svg>
		</a> -->
		<!-- Github -->
		<a
			class="rounded-lg text-gray-700 hover:bg-yellow-200 dark:text-gray-200
			dark:hover:bg-yellow-800"
			href={REPO_URL}
			aria-label="GitHub source"
		>
			<svg aria-hidden="true" class="h-9 w-9 p-1" fill="currentColor" viewBox="0 0 24 24">
				<path
					fill-rule="evenodd"
					d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483
					0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608
					1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088
					2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988
					1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112
					6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202
					2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566
					4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019
					10.019 0 0022 12.017C22 6.484 17.522 2 12 2z"
					clip-rule="evenodd"
				/>
			</svg>
		</a>
		<button
			aria-label="Toggle Dark Mode"
			class="ml-1 flex h-9 w-9 items-center justify-center rounded-lg bg-yellow-400 ring-yellow-400
			transition-all hover:ring-2 dark:bg-yellow-800"
			on:click={toggleDarkMode}
		>
			{#if isDark}
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					fill="none"
					stroke="currentColor"
					class="h-5 w-5 text-gray-800 dark:text-yellow-100"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728
						0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"
					/>
				</svg>
			{:else}
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					fill="none"
					stroke="currentColor"
					class="w-5 h-5 text-gray-800 dark:text-gray-200"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
					/>
				</svg>
			{/if}
		</button>
	</div>
</div>
</nav>

<style>
	.skip-nav {
		position: absolute;
		left: -25%;
		top: -2rem;
		--tw-translate-y: -3rem;
		padding: 0.75rem 1rem;
		transition-property: transform;
		transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
		transition-duration: 0.2s;
	}
</style>
