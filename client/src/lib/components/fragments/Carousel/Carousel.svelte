<script lang="ts">
	import type { ICarouselProps } from "$lib/types/carousel.types";

    // eslint-disable-next-line @typescript-eslint/no-empty-interface
    interface $$Props extends ICarouselProps{}

    export let images: $$Props['images'];
    $: imagesReactive = images;
    let carousel: HTMLElement;
    
    const updateCarousel = () => {
        const width: number = (carousel.childNodes.item(0) as Element).clientWidth;
        const carouselImages = document.querySelectorAll('.carouselImages');
        carousel.style.translate = `${-window.scrollY}px`;
        const firstImage: Element = carouselImages[0];
        if (firstImage.getBoundingClientRect().x <= -width) {
            const firstImageUrl: string | undefined = imagesReactive.shift();
            if (!firstImageUrl) return;
            imagesReactive.push(firstImageUrl);
            imagesReactive = imagesReactive;
            console.log(imagesReactive);
        }
    }
</script>

<div class="carousel-container" bind:this={carousel}>

    {#each imagesReactive as image, index}
    
    <img src={image} alt="ccet" class="carouselImages" />
    
    {/each}

</div>
<svelte:window on:scroll="{updateCarousel}"></svelte:window>

<style lang="scss">

    .carousel-container {
        position: absolute;
        left: -32px;
        display: flex;
        gap: 32px;
        height: 65vh;
        width: fit-content;
        // background-color: red;
        transform: rotate3d(1, 1, 1, 5deg) translate3d(0, 0, 0);
        overflow-x: scroll;
        scrollbar-width: none;  /* Firefox */
        -ms-overflow-style: none;  /* IE and Edge */
        &::-webkit-scrollbar {
            display: none;
        }
        scroll-behavior: smooth;
        scroll-snap-type: mandatory;

        img {
            image-rendering: optimizeSpeed;
            object-fit: cover;
            object-position: 0%;
            height: 100%;
            width: 50vw;
            border-radius: 16px;
        }
        scroll-snap-align: center;
    }

</style>
