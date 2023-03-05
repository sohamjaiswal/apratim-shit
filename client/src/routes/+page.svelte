<style lang="scss">
	.sect1 {
		display: flex;
		cursor: default;
	}

	.fore {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translateX(-50%) translateY(-50%);

		font-size: 2.5rem;
		text-align: center;
		text-shadow: 0 0 5px black;
	}

	.back {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translateX(-50%) translateY(-50%);
		width: 60%;
	}

	.logo {
		aspect-ratio: 1;
		width: 100%;
	}

	.mobileCarousel {
		display: none;
	}

	@media screen and (max-width: 600px) {
		.mobileCarousel {
			display: block;
		}
		.carousel {
			display: none;
		}
	}
	.fixed-wrapper {
		position: absolute;
		width: 100vw;
		height: 100%;
	}
</style>

<!-- Landing Page 🤌 -->
<script lang="ts">
	import Section from "$lib/components/ui/Section/Section.svelte";
	import Carousel from "$lib/components/fragments/Carousel/Carousel.svelte";
	import MobileCarousel from "$lib/components/fragments/mobileCarousel/MobileCarousel.svelte";
	import Page from "$lib/components/fragments/Page/Page.svelte";

	import logo from "$lib/assets/logo.png";

	let images = [
		"http://ccet.ac.in/tnp/images/banner11.jpg",
		"https://admissionadvice.in/wp-content/uploads/2020/11/Chandigarh-College-of-Engineering-and-Technology-CCET-Chandigarh.jpeg",
		// "https://ccet.ac.in/img/canteen/cafeteria-1.jpg",
		// "https://images.hindustantimes.com/rf/image_size_960x540/HT/p2/2018/02/22/Pictures/_3e183b5e-17b9-11e8-a4d7-7f47b8de9439.jpg"
	]
</script>

<Section>
	<div class="sect1">
		<div class="back">
			<img class="logo" src="{logo}" alt="apratim logo" />
		</div>
	</Section>
	<Section>
		<div class="carousel">
			<Carousel images={images} />
     </div>
   </Section>

