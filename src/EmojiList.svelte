<script>
  import Emoji from './Emoji.svelte';

  export let name;
  export let emojis;
</script>

<style>
  h3 {
    margin: 0;
    font-size: 0.9em;
    color: #333333;
  }

  .emoji-list {
    height: 10em;
    overflow: scroll;
  }
</style>

<h3>{name}</h3>

<div class="emoji-list">
  {#each emojis as emoji}
    <Emoji emoji={emoji} />
  {/each}
</div>