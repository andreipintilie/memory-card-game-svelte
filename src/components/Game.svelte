<script>
    import { onMount, createEventDispatcher } from "svelte";

    import Card from "./Card.svelte";
    import Matches from "./Matches.svelte";
    import emojis from "../utils/Emojis";
    import "../scss/main.scss";
    import Modal from "./Modal.svelte";

    export let possibleMatches;

    let myTimer;
    let timer = 0;
    let matches = 0;
    let totalTries = 0;
    let selectedEmojis = [];
    let firstCard, secondCard;
    let showGameStats = false;

    $: {
        if (showGameStats === false) {

        }
    }

    let formattedTime = '';

    $: {
        const minutes = Math.floor(timer / 60).toString().padStart(2, '0');
        const seconds = (timer % 60).toString().padStart(2, '0');
        formattedTime = `${minutes}:${seconds}`;
    }

    const dispatch = createEventDispatcher();

    const onGameOver = () => showGameStats = true;

    onMount(() => {
        getRandomEmojis();
    })

    function onCardClicked() {
        if (!myTimer) {
            myTimer = setInterval(() => {
                timer += 1;
            }, 1000)
        }

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

                    if (matches === possibleMatches) {
                        onGameOver();
                        clearInterval(myTimer);

                        matches = 0;
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
        selectedEmojis = shuffledEmojis.slice(0, possibleMatches).flatMap((emoji) => [emoji, emoji]);

        selectedEmojis = selectedEmojis.sort(() => Math.random() - 0.5);
    }

    const closeGameOverCard = (e) => {
        if (e.target !== e.currentTarget) return;
        
        if (e.target.querySelector('.card-element')) e.target.querySelector('.card-element').classList.add('fade-out')
        else e.target.parentNode.classList.add('fade-out');

        setTimeout(() => {
            dispatch('gameOver')
        }, 450);
    }
</script>

<Matches {matches} totalMatches={possibleMatches} {totalTries} timer={formattedTime} />

<div class="cards">
    {#each selectedEmojis as emoji, index}
        <Card face={emoji.emoji} cardClick={onCardClicked} order={index}></Card>
    {/each}
</div>

{#if showGameStats}
    <Modal onClickOutside={closeGameOverCard}>
        <h1 class="title">Game over</h1>
        <p>Time: {formattedTime}</p>
        <p>Number of attempts: {totalTries}</p>
        <p>Difficulty: {possibleMatches} possible matches</p>
        <button class="btn" on:click={closeGameOverCard}>Back to menu</button>
    </Modal>
{/if}
