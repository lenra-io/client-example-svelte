<script lang="ts">
	import { onMount } from "svelte";
	import { LenraApp } from "@lenra/client";

	import Counter from "./Counter.svelte";
	import welcome from "$lib/images/svelte-welcome.webp";
	import welcome_fallback from "$lib/images/svelte-welcome.png";

	let isConnected = false;
	let app: LenraApp;
	onMount(() => {
		app = new LenraApp({
			appName: "Example Client",
			clientId: "XXX-XXX-XXX",
		});

		app.connect().then(() => {
			console.log("Connected !");
			isConnected = true;
		});
	});
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		to your new<br />SvelteKit app
	</h1>

	<h2>
		try editing <strong>src/routes/+page.svelte</strong>
	</h2>

	{#if isConnected === true}
		<Counter {app} routeName="/counter/global" />
		<Counter {app} routeName="/counter/me" />
	{:else}
		<p>Loading...</p>
	{/if}
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
