<style lang="scss">
	section {
		height: 60vh;
		width: 100vw;
		overflow: visible;
		position: relative;
		transform: rotateZ(5deg);
		& > .image-track {
			display: flex;
			overflow-x: hidden;
			width: fit-content;
			gap: 4vmin;
			position: absolute;
			left: 50%;
			top: 50%;
			transform: translate(0%, -50%);
			user-select: none;
			& > img {
				width: 55vmin;
				height: 46vmin;
				object-fit: cover;
				object-position: center;
				border-radius: 18px;
			}
		}
	}
	@media screen and (max-width: 1024px) {
		section {
			height: 46vmin !important;
		}
		.image-track {
			left: 25% !important;
		}
	}
</style>

<script type="ts">
	import sampleImage from "$lib/assets/sample.webp";

	let track: HTMLElement;

	export let images: string[] = [
		sampleImage,
		sampleImage,
		sampleImage,
		sampleImage,
		sampleImage,
		sampleImage
	];

	const handleScroll = () => {
		return null;
	};

	const handleMouseMove = (e: MouseEvent | TouchEvent) => {
		if (!track.dataset.mouseDownAt || !track.dataset.prevPercentage) return;
		if (track.dataset.mouseDownAt === "0") return;
		const mouseDelta = parseFloat(track.dataset.mouseDownAt) - 
			((e as MouseEvent).clientX ?? (e as TouchEvent).touches[0].clientX),
			maxDelta = window.innerWidth / 2;
		const percentage = (mouseDelta / maxDelta) * -100;
		let nextPercentage = Math.max(
			Math.min(parseFloat(track.dataset.prevPercentage) + percentage, 0),
			-100);
		if (nextPercentage <= -75) {
			// eslint-disable-next-line no-undef
			track.childNodes.forEach((node: ChildNode) => {
				const clone: Node = node.cloneNode(true);	
				track.append(clone);
			});
		}
		track.dataset.percentage = `${nextPercentage}`;
		track.style.transform = `translate(${nextPercentage}%, -50%)`;
		track.animate(
			{
				transform: `translate(${nextPercentage}%, -50%)`
			},
			{ duration: 1200, fill: "forwards" }
		);

		for (const image of track.getElementsByClassName("image")) {
			image.animate(
				{
					objectPosition: `${100 + nextPercentage}% center`
				},
				{ duration: 1200, fill: "forwards" }
			);
		}
	};

	const handleMouseUp = () => {
		track.dataset.mouseDownAt = "0";
		track.dataset.prevPercentage = track.dataset.percentage;
	};

	const handleMouseDown = (e: MouseEvent | TouchEvent) => {
		track.dataset.mouseDownAt = `${((e as MouseEvent).clientX ?? (e as TouchEvent).touches[0].clientX)}`;
	};
</script>

<section>
	<div class="image-track" bind:this="{track}" data-mouse-down-at="0" data-prev-percentage="0"
		on:scroll="{handleScroll}"
		on:mousemove="{handleMouseMove}"
		on:touchmove="{handleMouseMove}"
		on:mouseup="{handleMouseUp}"
		on:touchend="{handleMouseUp}"
		on:mousedown="{handleMouseDown}"
		on:touchstart="{handleMouseDown}"
	>
		{#each images as image}
			<img src="{image}" alt="" class="" draggable="false" />
		{/each}
	</div>
</section>