<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	export let type: 'primary' | 'secondary' = 'primary';
	export let small = false;
	export let icon = false;
	export let href: string | undefined = undefined;
	export let external = false;
	export let loading = false;
	export let disabled = false;

	const dispatch = createEventDispatcher();

	function handleClick(event: MouseEvent) {
		if (!disabled && !loading) {
			dispatch('click', { event });
		}
	}
</script>

{#if href}
	<a
		class="button {type}"
		class:small
		class:icon
		class:loading
		class:disabled
		{href}
		target={external ? '_blank' : undefined}
		rel={external ? 'noreferrer' : undefined}
		on:click={handleClick}
	>
		<span class="content">
			<slot />
		</span>
	</a>
{:else}
	<button
		class="button {type}"
		class:small
		class:icon
		class:loading
		class:disabled
		on:click={handleClick}
	>
		<span class="content">
			<slot />
		</span>
		{#if loading}
			<span class="spinner" />
		{/if}
	</button>
{/if}

<style lang="scss">
	.button {
		--svg-size: 1.25rem;

		display: block;

		@include mixins.shiny(variables.$blue-2);
		@include mixins.shadow;

		&.secondary {
			@include mixins.shiny(variables.$grey-3);
		}

		&:not(.small) {
			@include typography.bold;

			padding: 0.75em 1em;

			.content {
				gap: 0.5em;
			}
		}

		&.small,
		&.icon {
			padding: 0.5rem;

			.content {
				gap: 0.25em;
			}
		}

		&.small {
			--svg-size: 1rem;
		}

		&.loading,
		&.disabled {
			cursor: not-allowed;
			opacity: 0.5;
		}

		&.loading .content {
			opacity: 0;
		}

		&:active .content {
			scale: 95%;
		}

		.content {
			display: flex;
			width: 100%;
			height: 100%;
			align-items: center;
			justify-content: center;
			transition: scale 0.05s ease-in-out;
			white-space: nowrap;

			:global(svg) {
				height: var(--svg-size);
				flex: 0 0 var(--svg-size);
			}
		}

		.spinner {
			position: absolute;
			width: 1.5rem;
			height: 1.5rem;
			border-radius: 50%;
			border-top: 2px solid;
			border-left: 2px solid;
			animation: spin 1s linear infinite;
			inset: 50%;

			@keyframes spin {
				0% {
					transform: translate(-50%, -50%) rotate(0deg);
				}

				100% {
					transform: translate(-50%, -50%) rotate(360deg);
				}
			}
		}
	}
</style>
