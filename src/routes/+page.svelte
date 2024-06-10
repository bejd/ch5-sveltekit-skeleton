<script>
	import { onMount } from "svelte";
	import Button from "./Button.svelte";

	const backgrounds = [
		"linear-gradient(65deg, #9d36e2, #facd38)",
		"linear-gradient(137deg, #0062ff, #61efff)",
		"linear-gradient(209deg, #62c579, #e4e25f)",
		"linear-gradient(281deg, #8262eb, #c990f0)",
		"linear-gradient(353deg, #f13b78, #f5e7ad)"
	];

	let CrComLib;
	let bgIndex = 0;

	// Wait for the window to be ready before importing CrComLib, then attach
	// functions to the window object.
	onMount(async () => {
		await import("@crestron/ch5-crcomlib/build_bundles/cjs/cr-com-lib.js")
			.then(response => CrComLib = response.CrComLib);

		window.CrComLib = CrComLib;
		window.bridgeReceiveIntegerFromNative = CrComLib.bridgeReceiveIntegerFromNative;
		window.bridgeReceiveBooleanFromNative = CrComLib.bridgeReceiveBooleanFromNative;
		window.bridgeReceiveStringFromNative = CrComLib.bridgeReceiveStringFromNative;
		window.bridgeReceiveObjectFromNative = CrComLib.bridgeReceiveObjectFromNative;
	});

	// The button will send a high signal to digital join ("b" for boolean) 21
	// when it's pressed, and a low signal when released.
	// For fun, if the signal is high, change the background gradient.
	function buttonPressed(signal) {
		CrComLib.publishEvent("b", "21", signal);

		if (signal) {
			if (bgIndex < backgrounds.length - 1) {
				bgIndex++;
			} else {
				bgIndex = 0;
			}
		}
	}
</script>

<main style:background={backgrounds[bgIndex]}>
	<h1>Crestron + Sveltekit 🥳</h1>

	<Button
		on:mousedown={ () => buttonPressed(true) }
		on:mouseup={ () => buttonPressed(false) }
	/>
</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		height: 100vh;
	}

	h1 {
		color: #eeeeee;

		font-size: 3em;
		font-family: Arial, Helvetica, sans-serif;
	}
</style>