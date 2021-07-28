<script>
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';

	export let percentageSeen, name, artistName, previousLink, nextLink, dataIndex, galleryObjects, previousObject,
		nextObject;


    let previousButton, nextButton

	const siteNavigation = (event) => {
		if (event.key === 'ArrowLeft') {
			if (dataIndex === 0) return;
			previousButton.click();
		}

		if (event.key === 'ArrowRight') {
			if (dataIndex === galleryObjects.length - 1) return;
			nextButton.click();
		}
	};

	onMount(() => {
		previousButton = document.querySelector('#previousLink');
		nextButton = document.querySelector('#nextLink');
	});

	$: percentageSeen = ((dataIndex + 1) / galleryObjects.length) * 100;

	const updatePreviousLink = (prevObject) => {
		if (prevObject && Object.keys(prevObject).length === 0 && prevObject.constructor === Object) {
			previousLink = '';
		} else {
			previousLink = prevObject.name.replace(/[\s]/gm, '-').toLowerCase();
		}
	};

	const updateNextLink = (nxtObject) => {
		if (nxtObject && Object.keys(nxtObject).length === 0 && nxtObject.constructor === Object) {
			nextLink = '';
		} else {
			nextLink = nxtObject.name.replace(/[\s]/gm, '-').toLowerCase();
		}
	};
	$: updatePreviousLink(previousObject);
	$: updateNextLink(nextObject);
</script>

<style lang="scss">
	@import '../../styles/mixins';

	footer {
		@include flex(0, space-between, 0);
		width: 100%;
		max-width: 144rem;
		@include py(1.6);
		position: relative;

		.piece-details {
			.name {
				font-weight: 700;
				font-size: 1.4rem;
				line-height: 1.7rem;
				margin-bottom: 0.9rem;
			}
			.artist-name {
				font-weight: normal;
				font-size: 1rem;
				line-height: 1.2rem;
				mix-blend-mode: normal;
				opacity: 0.75;
			}
		}

		nav {
			@include flex(0, 0, 0);
			gap: 2.3rem;
		}

		.top-line,
		.top-line-overlay {
			width: 100%;
			height: 1px;
			position: absolute;
			top: 0;
			left: 0;
			background: v(black);
		}

		.top-line {
			opacity: 0.25;
		}
	}
</style>

<svelte:window on:keyup={siteNavigation} />

<footer transition:fly={{x: -100, duration: 500}}>
	<div class="top-line" />
	<div
		class="top-line-overlay"
		style="width: {percentageSeen}%; transition: width 500ms cubic-bezier(.36,1.62,.78,.82);" />
	<div class="piece-details">
		<h2 class="name">{name}</h2>
		<h2 class="artist-name">{artistName}</h2>
	</div>
	<nav>
		<a
			id="previousLink"
			href="/gallery/{previousLink}"
			sveltekit:prefetch
			style={dataIndex === 0 ? 'pointer-events: none;' : ''}
			class:disabled={dataIndex === 0}>
			<svg width="26" height="24" xmlns="http://www.w3.org/2000/svg">
				<g stroke="#000" fill="none" fill-rule="evenodd">
					<path d="M24.166 1.843L3.627 12.113l20.539 10.269V1.843z" stroke-width="2" />
					<path fill="#D8D8D8" d="M.986.5h-1v22.775h1z" />
				</g>
			</svg>
		</a>
		<a
			id="nextLink"
			href="/gallery/{nextLink}"
			sveltekit:prefetch
			style={dataIndex === galleryObjects.length - 1 ? 'pointer-events: none;' : ''}
			class:disabled={dataIndex === galleryObjects.length - 1}>
			<svg width="26" height="24" xmlns="http://www.w3.org/2000/svg">
				<g stroke="#000" fill="none" fill-rule="evenodd">
					<path d="M1.528 1.843l20.538 10.27L1.528 22.382V1.843z" stroke-width="2" />
					<path fill="#D8D8D8" d="M24.708.5h1v22.775h-1z" />
				</g>
			</svg>
		</a>
	</nav>
</footer>
