<script context="module">
	import artData from '../../../static/data.json';

	const returnArtObjectIndex = (data) => {
		let slug = data;
		let artObjects = artData;
		let artObjectIndex = artObjects.findIndex((object) => {
			let tempObjectName = object.name.replace(/[\s]/gm, '-').toLowerCase();
			if (tempObjectName === slug) {
				return object;
			}
		});
		return artObjectIndex;
	};

	const returnArtObject = (data) => {
		let slug = data;
		let artObjects = artData;
		let artObject = artObjects.find((object) => {
			let tempObjectName = object.name.replace(/[\s]/gm, '-').toLowerCase();
			if (tempObjectName === slug) {
				return object;
			}
		});
		return artObject;
	};

	const returnPreviousObject = (index) => {
		let currentIndex = index;
		let artObjects = artData;
		let previousIndex = currentIndex - 1 === -1 ? false : currentIndex - 1;
		let previousObject = { ...artObjects.find((object, i) => i === previousIndex) };
		return previousObject;
	};
	const returnNextObject = (index) => {
		let currentIndex = index;
		let artObjects = artData;
		let nextIndex = currentIndex === artObjects.length - 1 ? false : currentIndex + 1;
		let nextObject = { ...artObjects.find((object, i) => i === nextIndex) };
		return nextObject;
	};

	export async function load({ page }) {
		const slug = page.params.slug;
		const dataObject = returnArtObject(slug);
		const dataIndex = returnArtObjectIndex(slug);
		const previousObject = returnPreviousObject(dataIndex);
		const nextObject = returnNextObject(dataIndex);
		return {
			props: {
				dataObject,
				slug,
				dataIndex,
				previousObject,
				nextObject
			}
		};
	}
</script>

<script>
	import { fade, fly } from 'svelte/transition';
	import Footer from '$lib/footer/Footer.svelte';
	import galleryObjects from '../../../static/data.json';
	import { getContext, onMount, onDestroy } from 'svelte';
	export let dataObject, slug, dataIndex, previousObject, nextObject;
	let artist,
		description,
		images,
		name,
		source,
		year,
		artistName,
		artistImage,
		previousButton,
		nextButton;
	let previousLink, nextLink;

	let footerData = {};

	const updateFooterData = (object) => {
		footerData = {
			name: object.name,
			artistName: object.artist.name,
			previousLink: previousLink,
			nextLink: nextLink,
			dataIndex: dataIndex,
			galleryObjects: [...galleryObjects],
			previousObject: previousObject,
			nextObject: nextObject
		};
	};

	$: updateFooterData(dataObject);

	const updateVariables = (object) => {
		artistName = object.artist.name;
		description = object.description;
		name = object.name;
		source = object.source;
		year = object.year;
		artistImage = object.artist.image;
	};

	let size = getContext('size');
	$: updateVariables(dataObject);
	$: imageSize = $size === 'mobile' ? 'small' : 'large';

	let showModal = false;

	const toggleModal = () => {
		showModal = !showModal;
	};

	const toggleEscListener = () => {
		const closeModalOnEsc = (e) => {
			if (e.key === 'Escape') {
				toggleModal();
			}
		};
		window.addEventListener('keyup', closeModalOnEsc);

		return {
			destroy() {
				window.removeEventListener('keyup', closeModalOnEsc);
			}
		};
	};

	let docLoaded = false;

	onMount(() => {
		docLoaded = true;
	});

	$: updateTitle(name);

	const updateTitle = (newName) => {
		if (docLoaded) {
			document.title = newName;
		}
	};
</script>

<style lang="scss">
	@import '../../styles/slug-styling';
	@import '../../styles/mixins';

	.view-image-button {
		background: v(black);
		mix-blend-mode: normal;
		opacity: 0.75;
		@include px(1.6);
		@include py(1.3);
		color: v(white);
		text-transform: uppercase;
		font-weight: bold;
		font-size: 1rem;
		line-height: 1.2rem;
		letter-spacing: 0.214286rem;
		@include flex(0, 0, 0);
		gap: 1.4rem;
		position: absolute;
		left: 1.6rem;
		top: 1.6rem;
		cursor: pointer;
		transition: all 250ms;

		&:hover {
			background: hsla(0, 0%, 100%, 0.25);
			opacity: 1;
		}

		@include mq(tablet) {
			top: unset;
			bottom: 1.6rem;
		}
	}

	a.disabled {
		opacity: 0.5;
	}

	.gallery-modal {
		position: fixed;
		@include ab-center();
		@include center();
		z-index: 500;

		.overlay {
			position: absolute;
			width: 100%;
			height: 100%;
			background: v(black);
			opacity: 0.85;
		}

		button {
			z-index: 501;
		}

		.modal-img-container {
			z-index: 501;
			padding: 2.4rem;
			position: relative;

			@include mq(desktop) {
				padding: 0;
			}

			button {
				position: absolute;
				top: -4.1rem;
				right: 0;
				text-transform: uppercase;
				font-size: 1.4rem;
				line-height: 1.7rem;
				letter-spacing: 0.3rem;
				color: v(white);
				padding: 0;
				margin: 0;
				transition: opacity 250ms;

				&:hover {
					opacity: 0.25;
				}
			}

			img {
				width: 100%;
				height: 100%;
				object-fit: cover;
			}
		}
	}
</style>

<svelte:head>
	<title>{name}</title>
</svelte:head>
<!-- Should be moved to its' own component, might do later -->
{#if showModal}
	<section class="gallery-modal" use:toggleEscListener>
		<div class="overlay" on:click={toggleModal} transition:fade={{ duration: 300 }} />
		<div class="modal-img-container" transition:fly={{ y: 100, duration: 400 }}>
			<button class="button">close</button>
			<img src="/static/assets/{slug}/gallery.jpg" alt={name} />
		</div>
	</section>
{/if}

<article class="gallery-item">
	<section class="work-image-container">
		<img
			src="../static/assets/{slug}/hero-{imageSize}.jpg"
			alt={name}
			transition:fly={{ x: -100, duration: 1100 }} />
		<button class="view-image-button" on:click={toggleModal} transition:fade={{ duration: 400 }}>
			<div class="img-container">
				<svg width="12" height="12" xmlns="http://www.w3.org/2000/svg">
					<g fill="#FFF" fill-rule="nonzero">
						<path
							d="M7.714 0l1.5 1.5-2.357 2.357 1.286 1.286L10.5 2.786l1.5 1.5V0zM3.857 6.857L1.5
							9.214 0 7.714V12h4.286l-1.5-1.5 2.357-2.357zM8.143 6.857L6.857 8.143 9.214 10.5l-1.5
							1.5H12V7.714l-1.5 1.5zM4.286 0H0v4.286l1.5-1.5 2.357 2.357 1.286-1.286L2.786 1.5z" />
					</g>
				</svg>
			</div>
			view image
		</button>
	</section>
	<section class="heading-container">
		<h1 transition:fly={{ y: -100, duration: 700 }}>{name}</h1>
		<h2 transition:fly={{ y: 100, duration: 900 }}>{artistName}</h2>
	</section>
	<section class="artist-img-container">
		<img
			src="../static/assets/{slug}/artist.jpg"
			alt={artistName}
			transition:fly={{ y: -100, x: 100, duration: 600 }} />
	</section>
	<section class="description-container">
		<h3 class="year" transition:fly={{ x: 100, duration: 600 }}>{year}</h3>
		<p class="description" transition:fly={{ x: -100, duration: 550 }}>{description}</p>
		<a href={source} class="source-link" transition:fade={{ duration: 700 }}>go to source</a>
	</section>
</article>

<Footer {...footerData} />
