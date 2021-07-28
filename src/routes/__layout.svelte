<script>

    import Header from '$lib/header.svelte'

	import { writable } from 'svelte/store';
	import { setContext } from 'svelte';
    import updateSize from '$lib/stores/size.js'

	let innerWidth;

	let size = writable();
	setContext('size', size);
    $: updateSize(size, innerWidth)
</script>

<svelte:window bind:innerWidth />


<style lang="scss">
	@import '../styles/mixins';
	.inner-body {
		width: 100%;
		height: auto;
		max-width: 136rem;
		@include px(2.4);
		padding-bottom: 4.7rem;
		transition: padding 250ms;
		@include flex(column nowrap, start, center);
		gap: 2.4rem;
		margin: auto;
		position: relative;

		@include mq(tablet){
			@include px(4);
			gap: 4rem;
		}

		@media screen and (min-width: 1400px){
			@include px(0);
		}
	}
</style>

<div class="inner-body">
	<Header />
	<slot />
</div>

