<slot />

<style>
	:global(.warning) {
		margin: 16px 0;
		padding: 8px 16px;
		background: rgba(214, 0, 0, 0.21);
		border-radius: 16px;
	}

	:global(.warning__paragraph) {
		margin: 0;
	}

	:global(.warning__link, .warning__link:visited) {
		color: white;
	}

	:global(.main-section) {
		box-sizing: border-box;
		padding: 0 32px;
	}

	:global(code) {
		color: rgba(255, 255, 255, 0.8);
	}

	@media only screen and (max-width: 800px) {
		:global(.main-section) {
			padding: 0 24px;
		}
	}

	@media only screen and (max-width: 320px) {
		:global(.main-section) {
			padding: 0 16px;
		}

		:global(.warning) {
			margin: 0 16px;
		}
	}
</style>
