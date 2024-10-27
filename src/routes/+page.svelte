<script>
	import Feed from './Feed/Feed.svelte';
	import Header from './Header.svelte';
	import Social from './Social.svelte';
	import meJpg from '$lib/images/me.jpg'
</script>
<svelte:head>
	<title>Mikstime's Personal Page</title>
	<meta name="description" content="Collection of articles" />
	<meta property="og:title" content="Michael Balitsky" />
	<meta property="og:type" content="website" />
	<meta property="og:description" content="Frontend related person" />
	<meta property="og:image" content={meJpg} />
	<meta property="og:url" content={meJpg} />
	<meta name="twitter:card" content="%sveltekit.assets%/me.jpg" />
	<meta name="keywords" content="Michael Balitsky, Михаил Балицкий, mikstime" />
</svelte:head>
<Header />
<Social />
<Feed />

<style>
</style>
