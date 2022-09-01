<script>
    let todos = [{text: 'first todo'},{text: 'second todo'}]
</script>

<div>
    <ul>
        {#each todos as {text}}
            <li>{text}</li>
        {/each}
    </ul>
</div>