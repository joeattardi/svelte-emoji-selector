<script>
  import { faSearch } from '@fortawesome/free-solid-svg-icons';
  import Icon from 'fa-svelte';
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
    top: calc(50% - 0.5em - 0.125em);
    right: 0.75em;
  }
</style>

<div class="svelte-emoji-picker__search">
  <input type="text" placeholder="Search emojis...">
  <span class="icon"><Icon icon={faSearch} /></span>
</div>