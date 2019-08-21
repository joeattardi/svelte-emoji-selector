<script>
  import Emoji from './Emoji.svelte';

  export let name = '';
  export let withTabs = true;
  export let emojis;
</script>

<style>
  h3 {
    margin: 0.25em;
    font-size: 0.9em;
    color: #333333;
  }

  .svelte-emoji-picker__emoji-list {
    height: 11rem;
    overflow: scroll;
  }

  .svelte-emoji-picker__emoji-list.tall {
    height: 14.9rem;
  }
</style>

<h3>{name}</h3>

<div class="svelte-emoji-picker__emoji-list" class:tall={!withTabs}>
  {#each emojis as emoji}
    <Emoji emoji={emoji} on:emojihover on:emojiclick />
  {/each}
</div>
