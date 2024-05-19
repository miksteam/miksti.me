<script>
	import Feed from './Feed.svelte';
	import Header from './Header.svelte';
	import Social from './Social.svelte';
	import me from '$lib/images/me.jpg'
</script>

<svelte:head>
	<title>Mikstime's Personal Page</title>
	<meta name="description" content="Collection of articles" />
	<meta property="og:title" content="Michael Balitsky" />
	<meta property="og:type" content="website" />
	<meta property="og:description" content="Frontend related person" />
	<meta property="og:image" content={me} />
	<meta property="og:url" content="https://miksti.me/" />
	<meta name="twitter:card" content={me} />
	<meta name="keywords" content="Michael Balitsky, Михаил Балицкий, mikstime">
</svelte:head>

<Header />
<Social />
<Feed />

<style>
</style>
