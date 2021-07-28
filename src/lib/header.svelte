<script>
	import galleryData from '../../static/data.json'
	let firstLink = galleryData[0].name.replace(/[\s]/gm,"-").toLowerCase()
</script>

<style lang="scss">
	@import '../styles/mixins';

	header {
		@include flex(0, space-between, 0);
		width: 100%;
		transition: padding 250ms;
        border-bottom: 0.1rem solid v(gray-200);
        @include py(2.4);

        @include mq(tablet){
            @include py(4);
        }

		.logo-container {
			height: 3.2rem;

			@include mq(desktop) {
				height: 4.8rem;
			}

			img {
				height: 100%;
			}
		}

		a {
			font-weight: bold;
			font-size: 1.2rem;
			line-height: 1.5rem;
			letter-spacing: 0.257143rem;
            color: v(gray-500);
            transition: color 250ms;
			text-transform: uppercase;
		}

        a:hover {
            color: v(black);
        }
	}
</style>

<header>
	<a class="logo-container" href="/">
		<img src="/assets/shared/logo.svg" alt="" />
	</a>
	<a href="/gallery/{firstLink}">start slideshow</a>
</header>
