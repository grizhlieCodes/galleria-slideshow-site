<script>
	import { onMount } from 'svelte';
	import { fly, fade } from 'svelte/transition';
	import gallery from '../../static/data.json';

	const updateGrid = (gridItems) => {
		gridItems.forEach((item) => {
			const image = item.querySelector('img');
			const rowHeight = 20; //js
			const gap = 20; // js
			const itemHeight = image.naturalHeight; // height of element before any manipulation
			const rowSpan = Math.ceil((itemHeight + gap) / (rowHeight + gap));
			item.style.gridRowEnd = 'span ' + rowSpan;
			const articleHeight = (rowHeight + gap) * (rowSpan - 1);
			item.style.height = articleHeight + 'px';
		});
	};

	let mounted = false;
	onMount(() => {
		mounted = true;
		const items = document.querySelectorAll('.art-piece');
		// updateGrid(items);
	});

	let oldWidth = 0;

	const checkIfWidthChanged = (newWidth) => {
		if (newWidth != oldWidth && mounted) {
			const items = document.querySelectorAll('.art-piece');
			oldWidth = newWidth;
			updateGrid(items);
		}
	};

	let innerWidth;

	const returnSlug = (name) => {
		let tempName = name;
		tempName = tempName.replace(/[\s]/gm, '-').toLowerCase();
		return tempName;
	};

	const randomMiliseconds = () => {
		let miliseconds = Math.random() * 1000;
		if (miliseconds < 400 && miliseconds > 900) {
			miliseconds = randomMiliseconds();
		}
		return Math.ceil(miliseconds);
	};
	const randomCoordinates = () => {
		let coordinates = Math.random() * 100;
		if (coordinates < 30 && coordinates > 120) {
			coordinates = randomCoordinates();
		}
		let decider = Math.round(Math.random()) * 2 - 1;
		if (decider == -1) {
			return Math.ceil(coordinates);
		} else {
			return 0 - Math.ceil(coordinates);
		}
	};
</script>

<style lang="scss">
	@import '../styles/mixins';
	article.gallery {
		height: 100%;
		width: 100%;
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(30rem, 1fr));
		gap: 2.4rem;

		@include mq(tablet) {
			grid-template-columns: repeat(auto-fit, minmax(30rem, 1fr));
		}
		@include mq(desktop) {
			gap: 4rem;
			grid-template-columns: repeat(4, 1fr);
		}

		.gallery-column {
			@include flex(column nowrap, space-between, center);
			gap: 2.4rem;
		}

		.art-piece {
			width: 100%;
			display: grid;
			grid: 'content' 1fr / 1fr;
			img {
				width: 100%;
				object-fit: cover;
			}

			* {
				grid-area: content;
			}

			.text-container {
				align-self: end;
				width: 100%;
				@include px(3.2);
				padding-bottom: 3.3rem;
				padding-top: 5.5rem;
				background: linear-gradient(180deg, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.841672) 100%);
				color: white;
				@include flex(column nowrap, start, start);
				gap: 0.7rem;

				h1 {
					// font-family: Libre Baskerville;
					// font-style: normal;
					font-weight: 700;
					font-size: 2.4rem;
					line-height: 3rem;
				}

				p {
					font-size: 1.3rem;
					line-height: 1.6rem;
				}
			}
		}
	}
</style>

<svelte:head>
	<title>Galleria.</title>
</svelte:head>
<svelte:window bind:innerWidth />
<article class="gallery" in:fade={{duration: 400, delay: 1000}} out:fade={{duration: 400}}>
	<section class="gallery-column">
		{#each gallery.filter((item) => item.gridColumn == '1/2') as { images, artist, name, gridColumn }, i}
			<a
				class="art-piece"
				href="/gallery/{returnSlug(name)}"
				sveltekit:prefetch>
				<img src="{images.thumbnail}" alt={name} />
				<div class="text-container">
					<h1 class="art-piece-name">{name}</h1>
					<p class="artist-name">{artist.name}</p>
				</div>
			</a>
		{/each}
	</section>
	<section class="gallery-column">
		{#each gallery.filter((item) => item.gridColumn == '2/3') as { images, artist, name, gridColumn }, i}
			<a class="art-piece" href="/gallery/{returnSlug(name)}" sveltekit:prefetch>
				<img src="{images.thumbnail}" alt={name} />
				<div class="text-container">
					<h1 class="art-piece-name">{name}</h1>
					<p class="artist-name">{artist.name}</p>
				</div>
			</a>
		{/each}
	</section>
	<section class="gallery-column">
		{#each gallery.filter((item) => item.gridColumn == '3/4') as { images, artist, name, gridColumn }, i}
			<a class="art-piece" href="/gallery/{returnSlug(name)}" sveltekit:prefetch>
				<img src="{images.thumbnail}" alt={name} />
				<div class="text-container">
					<h1 class="art-piece-name">{name}</h1>
					<p class="artist-name">{artist.name}</p>
				</div>
			</a>
		{/each}
	</section>
	<section class="gallery-column">
		{#each gallery.filter((item) => item.gridColumn == '4/5') as { images, artist, name, gridColumn }, i}
			<a class="art-piece" href="/gallery/{returnSlug(name)}" sveltekit:prefetch>
				<img src="{images.thumbnail}" alt={name} />
				<div class="text-container">
					<h1 class="art-piece-name">{name}</h1>
					<p class="artist-name">{artist.name}</p>
				</div>
			</a>
		{/each}
	</section>
</article>
