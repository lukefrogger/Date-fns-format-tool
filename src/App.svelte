<script>
	import format from "date-fns/format";
	import { fade } from "svelte/transition";
	import DarkMode from "svelte-dark-mode";
	import Toggle from "svelte-toggle";
	import { onMount } from "svelte";

	let error = false;
	let formattedDate = new Date().toISOString();
	let formatString = "";
	let showSuccess = false;

	let toggled = false;
	$: theme = toggled === true ? "dark" : "light";
	$: document.body.className = theme;

	const updateDateFormat = (e) => {
		try {
			formatString = e.target.value;
			if (formatString === "") {
				return (formattedDate = new Date().toISOString());
			}

			formattedDate = format(new Date(), formatString);
			error = false;
		} catch (err) {
			error = true;
		}
	};

	const copyToClipboard = (e) => {
		e.stopPropagation();
		navigator.clipboard.writeText(formatString);
		showSuccess = true;
		setTimeout(() => {
			showSuccess = false;
		}, 1000);
	};

	onMount(async () => {
		if (theme === "dark") {
			toggled = true;
		}
	});
</script>

<DarkMode bind:theme />

<main>
	<header>
		<div class="flex justify-between">
			<h1>Date-fns</h1>
			<div class="toggler">
				<div>
					<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"
						><!--! Font Awesome Pro 6.0.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path
							d="M256 159.1c-53.02 0-95.1 42.98-95.1 95.1S202.1 351.1 256 351.1s95.1-42.98 95.1-95.1S309 159.1 256 159.1zM509.3 347L446.1 255.1l63.15-91.01c6.332-9.125 1.104-21.74-9.826-23.72l-109-19.7l-19.7-109c-1.975-10.93-14.59-16.16-23.72-9.824L256 65.89L164.1 2.736c-9.125-6.332-21.74-1.107-23.72 9.824L121.6 121.6L12.56 141.3C1.633 143.2-3.596 155.9 2.736 164.1L65.89 256l-63.15 91.01c-6.332 9.125-1.105 21.74 9.824 23.72l109 19.7l19.7 109c1.975 10.93 14.59 16.16 23.72 9.824L256 446.1l91.01 63.15c9.127 6.334 21.75 1.107 23.72-9.822l19.7-109l109-19.7C510.4 368.8 515.6 356.1 509.3 347zM256 383.1c-70.69 0-127.1-57.31-127.1-127.1c0-70.69 57.31-127.1 127.1-127.1s127.1 57.3 127.1 127.1C383.1 326.7 326.7 383.1 256 383.1z"
						/></svg
					>
				</div>
				&nbsp;
				<span><Toggle hideLabel bind:toggled untoggledColor="#a7a7a7" toggledColor="#3168c133" /></span>
				&nbsp;
				<div>
					<svg viewBox="0 0 512 512"
						><!--! Font Awesome Pro 6.0.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path
							d="M32 256c0-123.8 100.3-224 223.8-224c11.36 0 29.7 1.668 40.9 3.746c9.616 1.777 11.75 14.63 3.279 19.44C245 86.5 211.2 144.6 211.2 207.8c0 109.7 99.71 193 208.3 172.3c9.561-1.805 16.28 9.324 10.11 16.95C387.9 448.6 324.8 480 255.8 480C132.1 480 32 379.6 32 256z"
						/></svg
					>
				</div>
			</div>
		</div>
		<h2>Formatting Tool</h2>
		<p><a href="https://date-fns.org/v2.28.0/docs/format" target="_blank">Date-fns format docs</a></p>
	</header>
	<div class="my">
		<label for="formattedDate">Format string</label>
		<div class="input-copy-wrapper">
			{#if showSuccess}<span class="completed-notif" transition:fade={{ duration: 100 }}>Copied</span>{/if}
			<input autofocus="autofocus" name="formattedDate" id="formattedDate" on:input={updateDateFormat} />
			<span class="icon-action" on:click={copyToClipboard}
				><svg viewBox="0 0 512 512"
					><!--! Font Awesome Pro 6.0.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path
						d="M384 96L384 0h-112c-26.51 0-48 21.49-48 48v288c0 26.51 21.49 48 48 48H464c26.51 0 48-21.49 48-48V128h-95.1C398.4 128 384 113.6 384 96zM416 0v96h96L416 0zM192 352V128h-144c-26.51 0-48 21.49-48 48v288c0 26.51 21.49 48 48 48h192c26.51 0 48-21.49 48-48L288 416h-32C220.7 416 192 387.3 192 352z"
					/></svg
				></span
			>
		</div>
	</div>
	<div class="my">
		<div class="result">{formattedDate}</div>
		{#if error}
			<div class="errorMessage" transition:fade={{ duration: 100 }}>Not a valid format</div>
		{/if}
	</div>
</main>

<style>
	main {
		padding: 1em;
		max-width: 500px;
		margin: 0 auto;
		width: 100%;
	}

	header {
		margin-bottom: 4rem;
		margin-top: 4rem;
	}

	.toggler {
		margin-top: 20px;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.toggler > div {
		width: 15px;
		height: 15px;
	}
	.toggler svg {
		width: 15px;
		fill: #333;
	}
	:global(body.dark) .toggler svg {
		fill: #ffffffb2;
	}

	.input-copy-wrapper {
		position: relative;
	}
	.input-copy-wrapper > span.completed-notif {
		position: absolute;
		top: 0px;
		left: 0px;
		height: 36px;
		width: 100%;
		overflow: hidden;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: #21cb3757;
		color: #017824;
		z-index: 999;
		border-radius: 3px;
	}
	:global(body.dark) .input-copy-wrapper > span.icon-action {
		background-color: #2888348c;
		color: #1fc934;
	}
	.input-copy-wrapper > span.icon-action {
		position: absolute;
		top: 1px;
		right: 1px;
		height: 34px;
		background: #e7e7e7;
		width: 45px;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 0px 3px 3px 0px;
		border-left: 1px solid #ccc;
		cursor: pointer;
	}

	:global(body.dark) .input-copy-wrapper > span.icon-action {
		background: #3168c17a;
		border-left: #3168c1;
	}

	.input-copy-wrapper > span.icon-action:active svg {
		width: 15px;
	}

	.input-copy-wrapper svg {
		transition: width 0.1s ease-out;
		width: 16px;
		margin-bottom: 1px;
		margin-left: 2px;
		fill: #606060;
	}
	:global(body.dark) .input-copy-wrapper svg {
		fill: #ffffffc0;
	}

	.result {
		font-size: 1.25rem;
	}

	.errorMessage {
		margin-top: 5px;
		color: #dd1717;
	}
</style>
