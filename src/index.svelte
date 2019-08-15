<script>
  import { onMount } from 'svelte';

  import { faBuilding, faFlag, faLightbulb, faSmile } from '@fortawesome/free-regular-svg-icons';
  import { faCat, faCoffee, faFutbol, faMusic } from '@fortawesome/free-solid-svg-icons';
  import Icon from 'fa-svelte';
  import Popper from 'popper.js';

  import ClickOutside from 'svelte-click-outside';
  import { Tabs, Tab, TabList, TabPanel } from 'svelte-tabs';

  import EmojiCategory from './EmojiCategory.svelte';

  import emojiData from './data/emoji.json';

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

  const emojiCategories = {};
  emojiData.forEach(emoji => {
    let categoryList = emojiCategories[emoji.category];
    if (!categoryList) {
      categoryList = emojiCategories[emoji.category] = [];
    }

    categoryList.push(emoji);
  });

  const categoryOrder = [
    'Smileys & People',
    'Animals & Nature',
    'Food & Drink',
    'Activities',
    'Travel & Places',
    'Objects',
    'Symbols',
    'Flags'
  ];

  const categoryIcons = {
    'Smileys & People': faSmile,
    'Animals & Nature': faCat,
    'Food & Drink': faCoffee,
    'Activities': faFutbol,
    'Travel & Places': faBuilding,
    'Objects': faLightbulb,
    'Symbols': faMusic,
    'Flags': faFlag
  };

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
    background: #FFFFFF;
    border: 1px solid #CCCCCC;
    border-radius: 5px;
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
    <Tabs> 
      <TabList>
        {#each categoryOrder as category}
          <Tab><Icon icon={categoryIcons[category]} /></Tab>
        {/each}
      </TabList>

      {#each categoryOrder as category}
        <TabPanel>
          <EmojiCategory name={category} />
        </TabPanel>
      {/each}
    </Tabs>
  </div>
</ClickOutside>