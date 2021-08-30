<script>
  const MIN_NUMBER = 2
  const MAX_NUMBER = 12
  const SECONDS_PER_TASK = 6
  const ROUNDS = 10

  import Numpad from '$lib/Numpad.svelte'

  let timer

  let task = ''
  let answer = ''

  let value = ''
  let score = 0
  let tries = ROUNDS + 1

  function play() {
    score = 0
    tries = ROUNDS + 1
    getTask()
  }

  function getTask() {
    function getRandomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min
    }

    value = ''
    tries--
    clearTimeout(timer)
    if (tries) timer = setTimeout(getTask, SECONDS_PER_TASK * 1000)

    const a = getRandomInt(MIN_NUMBER, MAX_NUMBER)
    const b = getRandomInt(MIN_NUMBER, MAX_NUMBER)
    const sign = a >= b ? (getRandomInt(0, 1) ? '-' : '+') : '+'

    task = `${a} ${sign} ${b}`
    answer = sign === '+' ? a + b : a - b
  }

  function handleSubmit() {
    if (parseInt(value) === answer) {
      score++
    }
    getTask()
  }
</script>

<div class="wrapper">
  {#if tries && task !== ''}
    <div id="score">
      Очки: {score}
    </div>
    <h1>{task}</h1>
    <h1 style="color: {value ? '#333' : '#ccc'}">{value || '?'}</h1>

    <Numpad bind:value on:submit={handleSubmit} />
  {:else}
    <div id="result">
      {#if score}
        Игра окончена.<br />
        Ваш результат: {score} из {ROUNDS}<br />
        {#if score < 6}
          Надо подучиться!
        {:else if score < 9}
          Хорошо!
        {:else}
          Молодчина!
        {/if}
      {/if}
      <button class="play-button" on:click={play}>Сыграем!</button>
    </div>
  {/if}
</div>

<style>
  .wrapper {
    text-align: center;
  }
  #result {
    text-align: center;
    font-size: 24px;
  }
  #score {
    margin-bottom: 1em;
    font-size: 24px;
  }
  .play-button {
    display: block;
    font-size: 24px;
    max-width: 10em;
    width: 100%;
    margin: 0 auto;
    margin-top: 2em;
  }
</style>
