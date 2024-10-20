<script lang="ts">
	import clipboardPng from './Feed.assets/clipboard.png';
	import clipboardWebp from './Feed.assets/clipboard.webp';

	import audiodelayPng from './Feed.assets/audiodelay.png';
	import audiodelayWebp from './Feed.assets/audiodelay.webp';

	import howIDesignedMyWebsitePng from './Feed.assets/how-i-designed-my-website.png';
	import howIDesignedMyWebsiteWebp from './Feed.assets/how-i-designed-my-website.webp';

	import sveltekitOverNextjsPng from './Feed.assets/sveltekit-over-nextjs.png';
	import sveltekitOverNextjsWebp from './Feed.assets/sveltekit-over-nextjs.webp';
</script>
<section class="feed">
	<h2 class="feed__title">MY LATEST ARTICLES</h2>
	<article class="feed__post">
		<a
			class="feed__post-link"
			target="_blank"
			rel="noopener"
			aria-label="Effortless and Secure Clipboard Magic with the Clipboard API. No."
			href="/demo/clipboard"
		>
			<picture>
				<source type="image/webp" srcset={clipboardWebp} />
				<img class="feed__post-img" src={clipboardPng} alt="" style="background-color: #212a44;"/>
			</picture>
		</a>
	</article>
	<article class="feed__post">
		<a
			class="feed__post-link"
			target="_blank"
			rel="noopener"
			aria-label="Challenges with AudioContext.outputLatency in Web Audio"
			href="/demo/audiodelay"
		>
			<picture>
				<source type="image/webp" srcset={audiodelayWebp} />
				<img class="feed__post-img" src={audiodelayPng} alt="" style="background-color: #2e315c;"/>
			</picture>
		</a>
	</article>
	<article class="feed__post">
		<a
			class="feed__post-link"
			target="_blank"
			rel="noopener"
			aria-label="How I Designed, Developed, and Deployed My Personal Website in an Evening (Mistakes Included)"
			href="https://www.linkedin.com/pulse/how-i-designed-developed-deployed-my-personal-website-balitsky-enmhe/?utm_source=share&utm_medium=guest_desktop&utm_campaign=copy"
		>
			<picture>
				<source type="image/webp" srcset={howIDesignedMyWebsiteWebp} />
				<img class="feed__post-img" src={howIDesignedMyWebsitePng} alt="" style="background-color: #732d2c;"/>
			</picture>
		</a>
	</article>
	<article class="feed__post">
		<a
			class="feed__post-link"
			target="_blank"
			rel="noopener"
			aria-label="Choosing SvelteKit Over Next.js for My Next Project"
			href="https://www.linkedin.com/pulse/choosing-sveltekit-over-nextjs-my-next-project-michael-balitsky-c3jte?utm_source=share&utm_medium=guest_desktop&utm_campaign=copy"
		>
			<picture>
				<source type="image/webp" srcset={sveltekitOverNextjsWebp} />
				<img class="feed__post-img" src={sveltekitOverNextjsPng} alt="" style="background-color: #161616;"/>
			</picture>
		</a>
	</article>
</section>

<style>
	.feed__title {
		margin: 0;
		padding-left: 32px;
		padding-bottom: 13px;
		font-weight: 700;
	}

	.feed__post {
		margin-bottom: 32px;
	}

	.feed__post-link {
		display: block;
		padding-bottom: 56.25%;
		background-size: cover;
		position: relative;
	}

	.feed__post-img {
		position: absolute;
		width: 100%;
		height: 100%;
		border-radius: 30px;
		pointer-events: none;
	}

	@media only screen and (max-width: 800px) {
		.feed__post {
			padding: 0 24px;
			margin-bottom: 24px;
		}

		.feed__title {
			padding-left: 24px;
		}

		.feed__post-link {
			border-radius: 15px;
		}
	}

	@media only screen and (max-width: 320px) {
		.feed__post {
			padding: 0 16px;
			margin-bottom: 16px;
		}
	}

	@media (hover: hover) {
		.feed__post {
			transition: transform 0.1s ease-in-out;
		}

		.feed__post:hover {
			transform: scale(1.01);
		}
	}
</style>
