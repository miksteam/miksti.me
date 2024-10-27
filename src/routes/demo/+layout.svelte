<script>
	import Header from '../Header.svelte';
</script>
<svelte:head>
	<title>123</title>
</svelte:head>
<Header href="/"/>
<div class="demo-page">
	<slot />
</div>
<style>
	:global(.callout) {
		margin: 16px 0;
		padding: 8px 16px;
		border-radius: 16px;
	}

	:global(.warning) {
		background: rgba(214, 0, 0, 0.21);
	}

	:global(.info) {
		background: rgba(69, 94, 101, 0.21);
	}
	:global(.callout__paragraph) {
		margin: 0;
	}

	:global(.link) {
		color: #5e67ff;
	}

	:global(.link:visited) {
		color: #5c31c9;
	}

	.demo-page {
		box-sizing: border-box;
		padding: 0 32px;
	}

	:global(code) {
		color: rgba(255, 255, 255, 0.8);
	}

	:global(.button) {
		padding: 8px;
		border: none;
		border-radius: 8px;
		background-color: white;
		font-size: 16px;
		color: black;
	}

	:global(tbody tr:nth-child(even)) {
		background-color: rgba(255, 255, 255, 0.1);
	}

	:global(.table__container) {
		width: 100%;
		overflow-x: auto;
	}
	:global(.table__container table) {
		width: 100%;
	}

	:global(.table__container td) {
		padding: 8px;
	}

	:global(table caption) {
		padding: 20px 0;
	}

	@media only screen and (max-width: 800px) {
		.demo-page {
			padding: 0 24px;
		}
	}

	@media only screen and (max-width: 320px) {
		.demo-page {
			padding: 0 16px;
		}

		:global(.warning) {
			margin: 0 16px;
		}
	}
</style>
