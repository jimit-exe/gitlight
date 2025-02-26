<script lang="ts">
	import { onMount } from 'svelte';
	import { page } from '$app/stores';
	import { PUBLIC_SITE_URL } from '$env/static/public';
	import { Button, Footer, IconButton, Tooltip } from '~/lib/components';
	import { ArrowRightIcon, GithubIcon, GitlabIcon } from '~/lib/icons';

	let onTauriApp = true;

	onMount(() => {
		onTauriApp = $page.url.search === '?desktop=true';
	});
</script>

<svelte:head>
	<title>GitLight • Log in</title>
</svelte:head>

<div class="wrapper">
	<img src="/images/large-light.webp" alt="" class="background-image" width="1600" height="384" />
	<main class="main">
		{#if !onTauriApp}
			<a href="/" class="back-button">
				<IconButton large>
					<ArrowRightIcon />
				</IconButton>
			</a>
		{/if}
		<h2 class="title">Log in to start monitoring your notifications</h2>
		<p class="description">You will be able to log in to the other provider afterward.</p>
		<span />
		<Button
			href={onTauriApp ? `${PUBLIC_SITE_URL}/auth/login?from_app=true` : '/auth/login'}
			external={onTauriApp && import.meta.env.PROD}
		>
			<GithubIcon />
			Log in to GitHub
		</Button>
		<Tooltip content="Coming soon!" position="bottom" hover>
			<Button disabled><GitlabIcon />Log in to GitLab</Button>
		</Tooltip>
	</main>
	<Footer />
</div>

<style lang="scss">
	.wrapper {
		position: relative;
		display: flex;
		overflow: hidden;
		height: 100vh;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 0 2rem;
	}

	.background-image {
		position: absolute;
		z-index: -1;
		top: 0;

		@include screens.mobile {
			width: 40rem;
			max-width: unset;
			height: auto;
		}
	}

	.main {
		position: relative;
		display: flex;
		width: 20rem;
		flex-direction: column;
		gap: 1rem;

		.back-button {
			position: absolute;
			bottom: calc(100% + 1rem);

			:global(svg) {
				rotate: 180deg;
			}
		}

		.title {
			@include typography.heading-1;
		}

		.description {
			color: variables.$grey-4;
		}
	}
</style>
