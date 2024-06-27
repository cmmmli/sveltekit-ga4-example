<script>
	import { browser } from '$app/environment';
	import { page } from '$app/stores';
	import { webVitals } from '$lib/vitals';
	import Header from './Header.svelte';
	import './styles.css';
	import { PUBLIC_GTM_ID } from '$env/static/public';
	import { onMount } from 'svelte';

	/** @type {import('./$types').LayoutServerData} */
	export let data;

	$: if (browser && data?.analyticsId) {
		webVitals({
			path: $page.url.pathname,
			params: $page.params,
			analyticsId: data.analyticsId
		});
	}

	onMount(() => {
		window.dataLayer = window.dataLayer || [];
		window.dataLayer.push({ 'gtm.start': new Date().getTime(), event: 'gtm.js' });
		const gtmScript = document.createElement('script');
		gtmScript.src = 'https://www.googletagmanager.com/gtm.js?id=' + PUBLIC_GTM_ID;
		document.head.append(gtmScript);
	});
</script>

<div class="app">
	<Header />

	<main>
		<slot />
	</main>

	<footer>
		<p>visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to learn SvelteKit</p>
	</footer>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 64rem;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 12px;
	}

	footer a {
		font-weight: bold;
	}

	@media (min-width: 480px) {
		footer {
			padding: 12px 0;
		}
	}
</style>
