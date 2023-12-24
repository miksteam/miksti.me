<header class="header">
	<div class="header__top">
		<h1 class="header__name">Michael Balitsky</h1>
	</div>
	<h2 class="header__description">Frontend related person</h2>
</header>

<style>
	.header__top {
		background-color: white;
		color: black;
		height: 10.4rem;
		display: flex;
		align-items: flex-end;
		box-sizing: border-box;
		padding: 0 32px;
	}

    .header__description {
        font-size: 2.4rem;
        padding-left: 32px;
		margin: 5px 0 32px;
        font-weight: 400;
    }

	.header__top::after {
		content: '';
		background-image: url($lib/images/me.jpg);
		background-size: cover;
		background-position: right;
		background-repeat: no-repeat;
		width: 9.6rem;
		height: 9.6rem;
		border-radius: 4.8rem;
		margin-bottom: -4.6rem;
		margin-left: auto;
        user-select: none;
	}

	.header__name {
		margin: 0;
		font-size: 3.6rem;
		padding-right: 8px;
        font-weight: 400;
	}

	@media only screen and (max-width: 800px) {
		.header__top {
			padding: 0 0 0 24px;
		}

		.header__top::after {
			height: 100%;
			margin-bottom: 0;
			border-radius: 0;
		}
        .header__name {
            font-size: 2.4rem;
        }

        .header__description {
            font-size: 1.2rem;
            padding-left: 24px;
            margin-bottom: 24px;
        }
	}

    @media only screen and (max-width: 320px) {
		.header__top {
			padding: 0 0 0 16px;
		}

        .header__description {
            font-size: 1.2rem;
            padding-left: 16px;
        }
	}
</style>
