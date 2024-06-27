<section class="social">
	<h2 class="a11y-hidden">Social Media</h2>
	<a
		target="_blank"
		rel="noopener"
		aria-label="telegram profile"
		class="social__link social__link_telegram_yes"
		href="https://t.me/mikstime/"
	>
	</a>
	<a
		target="_blank"
		rel="noopener"
		aria-label="linkedin profile"
		class="social__link social__link_linkedin_yes"
		href="https://www.linkedin.com/in/mikstime/"
	>
	</a>
</section>

<style>
	.a11y-hidden {
		font-weight: 400;
		height: 1px;
		left: auto;
		overflow: hidden;
		position: absolute;
		top: 0;
		width: 1px;
		clip: rect(1px 1px 1px 1px);
	}

	.social {
		display: flex;
		margin: 0;
		font-size: 3.6rem;
		padding: 0 8px 48px 32px;
	}

	.social__link {
		margin-right: 24px;
		display: block;
		width: 72px;
		height: 72px;
		background-size: 100%;
		background-repeat: no-repeat;
		background-position: center;
	}

	@media (hover: hover) {
		.social__link {
			transition: opacity 0.1s;
		}
		.social__link:hover {
			opacity: 0.9;
		}
	}

	.social__link_telegram_yes {
		background-image: url($lib/images/telegram.svg);
	}

	.social__link_linkedin_yes {
		background-image: url($lib/images/linkedin.svg);
	}

	@media only screen and (max-width: 800px) {
		.social__link {
			margin-right: 16px;
			width: 44px;
			height: 44px;
		}

		.social {
			padding-left: 24px;
			padding-bottom: 24px;
		}
	}

	@media only screen and (max-width: 320px) {
		.social {
			padding-left: 16px;
		}
	}
</style>
