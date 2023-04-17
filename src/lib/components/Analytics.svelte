<script lang="ts">
	import { onMount } from 'svelte';
	import { partytownSnippet } from '@builder.io/partytown/integration';

	// Add the Partytown script to the DOM head
	let partytownScriptEl: HTMLScriptElement;

	onMount(() => {
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
				// const siteUrl = `https://iseewhere.com/proxytown`;
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
	<!-- Insert `partytownSnippet` here -->
	<!-- Insert `partytownSnippet` here -->
	<!-- Insert `partytownSnippet` here -->
	<!-- Insert `partytownSnippet` here -->
	<script type="text/partytown" src="https://www.googletagmanager.com/gtag/js?id=G-9NHNR6ZQTS">
	</script>

	<!-- GTM script + config  -->

	<!-- prod: G-6W7F197GHD -->
	<!-- dev : G-9NHNR6ZQTS -->
	<!-- dev : G-9NHNR6ZQTS -->
	<!-- dev : G-9NHNR6ZQTS -->
	<!-- dev : G-9NHNR6ZQTS -->
	<script type="text/partytown">
		window.dataLayer = window.dataLayer || [];
		function gtag() {
			dataLayer.push(arguments);
		}
		gtag('js', new Date());
		gtag('config', 'G-9NHNR6ZQTS', {
			page_path: window.location.pathname
		});
	</script>
</svelte:head>
