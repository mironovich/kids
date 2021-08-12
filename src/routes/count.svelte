<script>
  const TIME = 5000
  const ROUNDS = 10
  const MAXNUMBER = 10

  import Numpad from '$lib/Numpad.svelte'

  let timer

  let task = ''
  let answer = ''

  let value = ''
  let score = 0
  let tries = ROUNDS + 1

  function getTask() {
    function getRandomInt(max) {
      return Math.floor(Math.random() * max)
    }

    value = ''
    score = 0
    tries = ROUNDS + 1
    tries--
    clearTimeout(timer)
    if (tries) timer = setTimeout(getTask, TIME)

    const a = getRandomInt(MAXNUMBER)
    const b = getRandomInt(MAXNUMBER)
    const sign = a >= b ? (getRandomInt(2) ? '-' : '+') : '+'

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
      <button class="play-button" on:click={getTask}>Сыграем!</button>
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
