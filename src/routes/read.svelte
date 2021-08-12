<script>
  import { onMount } from 'svelte'
  const consonants = [
    'Б',
    'В',
    'Г',
    'Д',
    'Ж',
    'З',
    'К',
    'Л',
    'М',
    'Н',
    'П',
    'Р',
    'С',
    'Т',
    'Ф',
    'Х',
    'Ц',
    'Ч',
    'Ш',
    'Щ',
  ]
  const vowels = ['А', 'Е', 'Ё', 'И', 'О', 'У', 'Ы', 'Э', 'Ю', 'Я']

  let slog = newSlog()

  function getRandomInt(max) {
    return Math.floor(Math.random() * Math.floor(max))
  }

  function newSlog() {
    return (
      consonants[getRandomInt(consonants.length)] +
      vowels[getRandomInt(vowels.length)]
    )
  }

  function handleInput({ key }) {
    // console.log('keydown ', key, '.')
    if (key === ' ' || !key) {
      slog = newSlog()
      // window.removeEventListener('keydown', handleInput)
    }
  }

  onMount(() => window.addEventListener('keydown', handleInput))
</script>

<div class="container" on:click={handleInput}>
  <p class="slog">{slog}</p>
</div>

<style>
  .container {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .slog {
    color: #000000;
    font-size: 15rem;
    font-weight: bold;
    font-family: 'Roboto';
  }
</style>
