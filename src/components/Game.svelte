<script>
    import { onMount, createEventDispatcher } from "svelte";

    import Card from "./Card.svelte";
    import Matches from "./Matches.svelte";
    import emojis from "../utils/Emojis";
    import "../scss/main.scss";

    export let chosenMatches;

    let matches = 0;
    let totalTries = 0;
    let selectedEmojis = [];
    let firstCard, secondCard;

    const dispatch = createEventDispatcher();

    const onGameOver = () => dispatch('gameOver');

    onMount(() => {
        getRandomEmojis();
    })

    function onCardClicked() {
        const clickedCard = this;

        if (firstCard && secondCard) return;

        if (!firstCard) {
            if (firstCard === clickedCard) return;
            if (clickedCard.classList.contains('active')) return;

            firstCard = clickedCard

            clickedCard.classList.add('active')
        } else {
            if (clickedCard === firstCard) return;
            if (clickedCard.classList.contains('active')) return;

            secondCard = clickedCard

            clickedCard.classList.add('active')

            // If cards have the same value
            const firstCardFace = firstCard.querySelector('.card-inner-back').textContent;
            const secondCardFace = secondCard.querySelector('.card-inner-back').textContent;

            if (firstCardFace === secondCardFace) {
                matches++;
                totalTries++;

                setTimeout(() => {
                    firstCard.classList.add('hidden')
                    secondCard.classList.add('hidden')
                }, 500)

                setTimeout(() => {
                    firstCard = undefined;
                    secondCard = undefined;

                    if (matches === chosenMatches) {
                        matches = 0;
                        chosenMatches = 0;

                        onGameOver();
                    }
                }, 1000)
            } else {
                // if cards don't match
                setTimeout(() => {
                    totalTries++;

                    firstCard.classList.remove('active');
                    secondCard.classList.remove('active');

                    firstCard.classList.add('shake');
                    secondCard.classList.add('shake');
                }, 500)
                setTimeout(() => {
                    firstCard.classList.remove('shake');
                    secondCard.classList.remove('shake');

                    firstCard = undefined;
                    secondCard = undefined;
                }, 1000)
            }
        }
    }

    const getRandomEmojis = () => {
        const shuffledEmojis = emojis.sort(() => Math.random() - 0.5);
        selectedEmojis = shuffledEmojis.slice(0, chosenMatches).flatMap((emoji) => [emoji, emoji]);

        selectedEmojis = selectedEmojis.sort(() => Math.random() - 0.5);
    }

</script>


<Matches {matches} totalMatches={chosenMatches} {totalTries} />

<div class="cards">
{#each selectedEmojis as emoji, index}
    <Card face={emoji.emoji} cardClick={onCardClicked} order={index}></Card>
{/each}
</div>