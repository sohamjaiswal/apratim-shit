<script lang="ts">
	import type { IGlossyCard, IGlossyCards } from '../../../types/glossy-card.types';

    // eslint-disable-next-line @typescript-eslint/no-empty-interface
    interface $$Props extends IGlossyCards{}

    export let cards: IGlossyCard[] = Array(6)

    cards.forEach((card) => {
        console.log(card.link)
    })

    const handleContainerMouseMove = (e: any) => {
        //eslint-disable-next-line no-undef
        const cards: HTMLCollectionOf<Element> = document.getElementsByClassName('card')
        //eslint-disable-next-line no-undef
        const realCards = cards as HTMLCollectionOf<HTMLElement>
        if (realCards) {
            for (let i = 0; realCards.length; i++) {
            const rect = realCards[i].getBoundingClientRect(),
                x = e.clientX - rect.left,
                y = e.clientY - rect.top

            realCards[i].style.setProperty("--mouse-x", `${x}px`)
            realCards[i].style.setProperty("--mouse-y", `${y}px`)
        }
        }
    }
</script>

<div class="cards" on:mousemove="{handleContainerMouseMove}">
    {#each cards as cardContent}
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class="card" on:click={() => {cardContent.link ? window.open(cardContent.link, '_blank') : null}} class:clickable={cardContent.link}>
        <div class="card-border" />
        <div class="card-content">
            <div class="banner" style={`background-image: url(${cardContent.image});`} />
            <div class="content">
                {@html cardContent.description}
            </div>
        </div>
    </div>
    {/each}
</div>

<style lang="scss">
    .clickable {
        cursor: pointer;
    }

    .cards {

        display: flex;

        flex-wrap: wrap;

        width: 100%;
        gap: 8px;

        justify-content: center;

        .card {
            background-color: rgba(255, 255, 255, 0.1);
            height: 260px;
            width: 300px;
            border-radius: 8px;
            position: relative;

            &::before, .card-border {
                border-radius: inherit;
                content: "";
                height: 100%;
                position: absolute;
                left: 0px;
                top: 0px;
                opacity: 0;
                transition: opacity 500ms;
                width: 100%;
                z-index: 2;
            }

            &::before {
                background: radial-gradient(
                    800px circle at var(--mouse-x) var(--mouse-y),
                    rgba(255,255,255,0.06),
                    transparent 40%
                );
                z-index: 3;
            }

            .card-border {
                background: radial-gradient(
                    400px circle at var(--mouse-x) var(--mouse-y),
                    rgba(255,255,255,0.3),
                    transparent 40%
                );
                z-index: 1;
            }

            &:hover::before{
                opacity: 1;
            }

            > .card-content {
                height: calc(100% - 2px);
                width: calc(100% - 2px);
                background-color: var(--card-bg);
                border-radius: inherit;
                margin: 1px;
                position: relative;
                z-index: 2;
                .banner {
                    position: absolute;
                    height: inherit;
                    top: 0;
                    left: 0;
                    right: 0;
                    background-size: contain;
                    background-position: center top;
                    background-repeat: no-repeat;
                    z-index: 4;
                    border-radius: inherit;
                    box-shadow: inset 0px -150px 40px 0px var(--card-bg);
                }
                .content {
                    margin: 55% 16px;
                    position: absolute;
                    z-index: 5;
                }
            }
        }
        &:hover > .card > .card-border {
            opacity: 1;
        }
    }
</style>