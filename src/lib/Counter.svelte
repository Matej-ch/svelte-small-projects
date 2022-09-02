<script>
  import {fly} from "svelte/transition"

  export let title = 'Cool title';

  let count = 0
  const increment = () => {
    count += 1.3
  }

  const decrement = () => {
    count -= 1.1
  }

  $: doubled = count * 2;

  let textInput = '';
</script>
<h2>{title}</h2>
<div>

  {#key count}
    <span class="inline-block" in:fly={{y:-20}}>{count.toFixed(2)} </span> and doubled is
    <span class="inline-block" in:fly={{y:-20}}>{doubled.toFixed(2)}</span>
  {/key}


  {#if count > 5}
    power of 2 of count is

    <span class="inline-block" in:fly={{y:-20}}>
      {Math.pow(count, 2).toFixed(2)}
    </span>

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