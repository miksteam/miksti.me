<script>
	import image from '$lib/images/image.jpeg';
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<img src={image} alt="" />
	<h1>Михаил Балицкий</h1>
	<a href="https://t.me/mikstime"> t.me/mikstime </a>
</section>

<style>
	section {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		min-height: 100%;
	}

	a:visited,
	a {
		color: white;
		text-decoration: none;
		font-size: 2rem;
	}

	h1 {
		font-size: 3rem;
		text-align: center;
		height: 5rem;
	}

	img {
		height: 70vw;
		border-radius: 50%;
		pointer-events: none;
		user-select: none;
		margin-top: 2rem;
	}
</style>
