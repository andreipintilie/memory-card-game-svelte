<script>
  import Card from "./components/Card.svelte";
  import Matches from "./components/Matches.svelte";
  import emojis from "./utils/Emojis";
  import "./scss/main.scss";

  let matches = 0;
  let totalMatches = 0;
  let selectedEmojis = [];
  let firstCard, secondCard;

  const onCardClicked = (e) => {
    const clickedCard = e.target.parentNode.parentNode;

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

        firstCard = undefined;
        secondCard = undefined;
      } else {
        // if cards don't match
        setTimeout(() => {
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

  function getRandomEmojis() {
    const shuffledEmojis = emojis.sort(() => Math.random() - 0.5);
    selectedEmojis = shuffledEmojis.slice(0, 6).flatMap((emoji) => [emoji, emoji]);

    totalMatches = selectedEmojis.length / 2;
    selectedEmojis = selectedEmojis.sort(() => Math.random() - 0.5);
  }

  getRandomEmojis();
</script>

<main>
  <Matches {matches} {totalMatches} />

  <div class="cards">
    {#each selectedEmojis as emoji}
      <Card face={emoji.emoji} cardClick={onCardClicked}></Card>
    {/each}
  </div>
</main>
