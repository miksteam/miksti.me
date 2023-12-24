<section>
	<h3 class="feed__title">MY LATEST ARTICLE</h3>
	<div class="feed__post">
		<a
			class="feed__post-link"
            target="_blank"
            rel="noopener"
            aria-label="Choosing SvelteKit Over Next.js for My Next Project"
			href="https://www.linkedin.com/pulse/choosing-sveltekit-over-nextjs-my-next-project-michael-balitsky-c3jte?utm_source=share&utm_medium=guest_desktop&utm_campaign=copy"
		>
		</a>
	</div>
</section>

<style>
	.feed__title {
		margin: 0;
		padding-left: 32px;
		font-weight: 700;
	}

    .feed__post {
		padding: 13px 0;
    }

	.feed__post-link {
		display: block;
		padding-bottom: 56.25%;
		background-image: url($lib/images/latest.png);
		background-size: cover;
		border-radius: 30px;
	}

	@media only screen and (max-width: 800px) {
        .feed__post {
		    padding: 13px 24px;
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
		    padding: 13px 16px;
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
