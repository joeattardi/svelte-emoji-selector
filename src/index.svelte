<script>
  import { onMount } from 'svelte';

  import { faBuilding, faFlag, faLightbulb, faSmile } from '@fortawesome/free-regular-svg-icons';
  import { faCat, faCoffee, faFutbol, faMusic } from '@fortawesome/free-solid-svg-icons';
  import Icon from 'fa-svelte';
  import Popper from 'popper.js';

  import ClickOutside from 'svelte-click-outside';
  import { Tabs, Tab, TabList, TabPanel } from 'svelte-tabs';

  import EmojiDetail from './EmojiDetail.svelte';
  import EmojiList from './EmojiList.svelte';

  import emojiData from './data/emoji.js';

  const smileIcon = faSmile;

  let triggerButtonEl;
  let pickerEl;
  let popper;

  let pickerVisible = false;

  let currentEmoji;

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

  console.log(emojiCategories);

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

  function showEmojiDetails(event) {
    currentEmoji = event.detail;
  }
</script>

<style>
  .svelte-emoji-picker {
    background: #FFFFFF;
    border: 1px solid #CCCCCC;
    border-radius: 5px;
    width: 22rem;
    margin: 0 0.5em;
    box-shadow: 0px 0px 3px 1px #CCCCCC;
  }

  .svelte-emoji-picker__trigger {
    cursor: pointer;
  }

  .svelte-emoji-picker__emoji-tabs {
    padding: 0.25em;
  }

  :global(.svelte-emoji-picker__emoji-tabs .svelte-tabs ul.svelte-tabs__tab-list) {
    display: flex;
  }

  :global(.svelte-emoji-picker__emoji-tabs .svelte-tabs li.svelte-tabs__tab) {
    flex-grow: 1;
  }
</style>

<button class="svelte-emoji-picker__trigger" bind:this={triggerButtonEl} on:click={togglePicker}>
  <Icon icon={smileIcon} />
</button>

<ClickOutside on:clickoutside={hidePicker} exclude={[triggerButtonEl]}>
  <div class="svelte-emoji-picker" bind:this={pickerEl} hidden={!pickerVisible}>
    <div class="svelte-emoji-picker__emoji-tabs">
      <Tabs> 
        <TabList>
          {#each categoryOrder as category}
            <Tab><Icon icon={categoryIcons[category]} /></Tab>
          {/each}
        </TabList>

        {#each categoryOrder as category}
          <TabPanel>
            <EmojiList name={category} emojis={emojiCategories[category]} on:emojihover={showEmojiDetails} on:emojiclick />
          </TabPanel>
        {/each}
      </Tabs>
    </div>

    <EmojiDetail emoji={currentEmoji} />
  </div>
</ClickOutside>