<script>
  export let title='Cool title';

  let count = 0
  const increment = () => {
    count += 1.3
  }

  const decrement = () => {
    count -= 1.1
  }
</script>

<button on:click={increment}>
  count with name {title} is {count.toFixed(2)}
</button>

<button on:click={decrement}>
  count with name {title} is {count.toFixed(2)}
</button>
