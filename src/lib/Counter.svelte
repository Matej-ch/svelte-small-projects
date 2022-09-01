<script>
  export let title='Cool title';

  let count = 0
  const increment = () => {
    count += 1.3
  }

  const decrement = () => {
    count -= 1.1
  }

  $: doubled = count *2;

  let textInput = '';
</script>
<h2>{title}</h2>
<div>
  {count.toFixed(2)} and doubled is {doubled.toFixed(2)}

  {#if count > 5}
    power of 2 of count is {Math.pow(count,2).toFixed(2)}
  {/if}

</div>

<button on:click={increment}>
  +
</button>

<button on:click={decrement}>
  -
</button>

<div>
  <form action="#">
    <input type="text" placeholder="input text" bind:value={textInput}>
    <div>{textInput}</div>
  </form>
</div>