<section class="feed">
	<h3 class="feed__title">MY LATEST ARTICLES</h3>
	<article class="feed__post feed__post_1">
		<a
			class="feed__post-link"
            target="_blank"
            rel="noopener"
            aria-label="How I Designed, Developed, and Deployed My Personal Website in an Evening (Mistakes Included)"
			href="https://www.linkedin.com/pulse/how-i-designed-developed-deployed-my-personal-website-balitsky-enmhe/?utm_source=share&utm_medium=guest_desktop&utm_campaign=copy"
		>
		</a>
	</article>
	<article class="feed__post feed__post_2">
		<a
			class="feed__post-link"
            target="_blank"
            rel="noopener"
            aria-label="Choosing SvelteKit Over Next.js for My Next Project"
			href="https://www.linkedin.com/pulse/choosing-sveltekit-over-nextjs-my-next-project-michael-balitsky-c3jte?utm_source=share&utm_medium=guest_desktop&utm_campaign=copy"
		>
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
		border-radius: 30px;
	}

	.feed__post_1 .feed__post-link {
		background-image: url($lib/images/how-i-designed-my-website.png);
	}

	.feed__post_2 .feed__post-link {
		background-image: url($lib/images/sveltekit-over-nextjs.png);
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
            transition: transform .1s ease-in-out;
        }

        .feed__post:hover {
            transform: scale(1.01);
        }
    }
</style>
