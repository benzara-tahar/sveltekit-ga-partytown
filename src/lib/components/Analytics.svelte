<script lang="ts">
	import { onMount } from 'svelte';

	import { partytownSnippet } from '@builder.io/partytown/integration';
	import { page } from '$app/stores';

	// Add the Partytown script to the DOM head
	let partytownScriptEl: HTMLScriptElement;

	onMount(() => {
		console.log($page.url);
		if (partytownScriptEl) {
			partytownScriptEl.textContent = partytownSnippet();
		}
	});
</script>

<svelte:head>
	<script>
		// Forward the necessary functions to the web worker layer
		partytown = {
			forward: ['dataLayer.push'],
			resolveUrl: (url) => {
				const siteUrl = `https://sveltekit-ga-partytown.vercel.app/proxytown`;

				if (url.hostname === 'www.googletagmanager.com') {
					const proxyUrl = new URL(`${siteUrl}/gtm`);

					const gtmId = new URL(url).searchParams.get('id');
					gtmId && proxyUrl.searchParams.append('id', gtmId);

					return proxyUrl;
				} else if (url.hostname === 'www.google-analytics.com') {
					const proxyUrl = new URL(`${siteUrl}/ga`);

					return proxyUrl;
				}

				return url;
			}
		};
	</script>

	<script bind:this={partytownScriptEl}></script>
	<!-- conditionally load google analytics ( on production only for example) -->

	{#if $page.url.hostname === 'sveltekit-ga-partytown.vercel.app'}
		<script type="text/partytown" src="https://www.googletagmanager.com/gtag/js?id=<YOUR-ID-HERE>">
		</script>

		<!-- GTM script + config  -->
		<script type="text/partytown">
			window.dataLayer = window.dataLayer || [];
			function gtag() {
				dataLayer.push(arguments);
			}
			gtag('js', new Date());
			gtag('config', '<YOUR-ID-HERE>', {
				page_path: window.location.pathname
			});
		</script>
	{/if}
</svelte:head>
