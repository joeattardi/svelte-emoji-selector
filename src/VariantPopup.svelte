<script>
	import { createEventDispatcher } from 'svelte';

  import { faTimes } from '@fortawesome/free-solid-svg-icons';
  import Icon from 'fa-svelte';

  import Emoji from './Emoji.svelte';

  export let variants;

  const dispatch = createEventDispatcher();

  function onClickClose() {
    dispatch('close');
  }
  
  function onClickContainer(event) {
    dispatch('close');
  }
</script>

<style>
  .svelte-emoji-picker__variants-container {
    position: absolute;
    top: 0;
    left: 0;
    background: rgba(0, 0, 0, 0.5);
    width: 23rem;
    height: 21rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .svelte-emoji-picker__variants {
    background: #FFFFFF;
    margin: 0.5em;
    padding: 0.5em;
    text-align: center;
  }

  .svelte-emoji-picker__variants .close-button {
    position: absolute;
    font-size: 1em;
    right: 0.75em;
    top: calc(50% - 0.5em);
    cursor: pointer;
  }
</style>

<div class="svelte-emoji-picker__variants-container" on:click={onClickContainer}>
  <div class="svelte-emoji-picker__variants">
    {#each Object.keys(variants) as variant}
      <Emoji emoji={variants[variant]} on:emojiclick />
    {/each}
    <div class="close-button" role="button" on:click={onClickClose}>
      <Icon icon={faTimes} />
    </div>
  </div>
</div>
