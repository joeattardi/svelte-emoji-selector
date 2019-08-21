<script>
  import { onMount } from 'svelte';

  import { faSearch, faTimes } from '@fortawesome/free-solid-svg-icons';
  import Icon from 'fa-svelte';

  export let searchText = '';

  let searchField;

  onMount(() => {
    searchField.focus();
  });

  function clearSearchText() {
    searchText = '';
    searchField.focus();
  }

  function handleKeyDown(event) {
    if (event.key === 'Escape' && searchText) {
      clearSearchText();
      event.stopPropagation();
    }
  }
</script>

<style>
  .svelte-emoji-picker__search {
    padding: 0.25em;
    position: relative;
  }

  .svelte-emoji-picker__search input {
    width: 100%;
    border-radius: 5px;
  }

  .svelte-emoji-picker__search input:focus {
    outline: none;
    border-color: #4F81E5;
  }

  .icon {
    color: #AAAAAA;
    position: absolute;
    font-size: 1em;
    top: calc(50% - 0.5em);
    right: 0.75em;
  }

  .icon.clear-button {
    cursor: pointer;
  }
</style>

<div class="svelte-emoji-picker__search">
  <input type="text" placeholder="Search emojis..." bind:value={searchText} bind:this={searchField} on:keydown={handleKeyDown}>
  
  {#if searchText}
    <span class="icon clear-button" role="button" on:click|stopPropagation={clearSearchText}><Icon icon={faTimes} /></span>
  {:else}
    <span class="icon"><Icon icon={faSearch} /></span>
  {/if}
</div>
