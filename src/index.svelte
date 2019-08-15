<script>
  import { onMount } from 'svelte';

  import { faSmile } from '@fortawesome/free-regular-svg-icons';
  import Icon from 'fa-svelte';
  import Popper from 'popper.js';

  import ClickOutside from 'svelte-click-outside';

  const smileIcon = faSmile;

  let triggerButtonEl;
  let pickerEl;
  let pickerVisible = false;
  let popper;

  onMount(() => {
    popper = new Popper(triggerButtonEl, pickerEl, {
      placement: 'right'
    });
  });

  function onClickOutside(event) {
    const buttonParent = event.target.closest('.emoji-picker__trigger');
    const pickerParent = event.target.closest('.emoji-picker');

    if (!buttonParent && !pickerParent) {
      pickerVisible = false;
    }
  }

  function hidePicker() {
    pickerVisible = false;
  }

  function togglePicker() {
    pickerVisible = !pickerVisible;

    if (pickerVisible) {
      popper.update();
    }
  }
</script>

<style>
  .emoji-picker {
    background: #AAAAAA;
    width: 20em;
    padding: 0.5em;
    margin: 0 0.5em;
  }

  .emoji-picker__trigger {
    cursor: pointer;
  }
</style>

<button class="emoji-picker__trigger" bind:this={triggerButtonEl} on:click={togglePicker}>
  <Icon icon={smileIcon} />
</button>

<ClickOutside on:clickoutside={hidePicker} exclude={[triggerButtonEl]}>
  <div class="emoji-picker" bind:this={pickerEl} hidden={!pickerVisible}>
    Emoji Picker
    <span>click me</span>
  </div>
</ClickOutside>