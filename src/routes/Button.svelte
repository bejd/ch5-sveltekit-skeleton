<button
	type="button"
	on:mousedown
	on:mouseup
	>
	Change background!
</button>

<style>
	button {
		border: none;
		border-radius: 1em;
		padding: 1em;

		background-color: #eeeeee;
		color: #333333;

		font-size: 1.5em;

		transition: transform 0.1s;
	}

	button:active {
		transform: scale(0.95);
	}
</style>