<script>
  export let emoji;
</script>

<style>
  button {
    border: none;
    background: transparent;
    cursor: pointer;
    font-size: 1.3em;
    width: 1.5em;
    height: 1.5em;
    padding: 0;
    margin: 0;
  }

  button:hover {
    background: #E8F4F9;
    border-radius: 5px;
  }
</style>

<button>{emoji.emoji}</button>