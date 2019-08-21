<script>
  import { createEventDispatcher, onMount, tick } from 'svelte';

  import { faBuilding, faFlag, faLightbulb, faSmile } from '@fortawesome/free-regular-svg-icons';
  import { faCat, faCoffee, faFutbol, faMusic } from '@fortawesome/free-solid-svg-icons';
  import Icon from 'fa-svelte';
  import Popper from 'popper.js';

  import ClickOutside from 'svelte-click-outside';
  import { Tabs, Tab, TabList, TabPanel } from 'svelte-tabs';

  import EmojiDetail from './EmojiDetail.svelte';
  import EmojiList from './EmojiList.svelte';
  import EmojiSearch from './EmojiSearch.svelte';
  import EmojiSearchResults from './EmojiSearchResults.svelte';
  import VariantPopup from './VariantPopup.svelte';

  import emojiData from './data/emoji.js';

  const smileIcon = faSmile;

  let triggerButtonEl;
  let pickerEl;
  let popper;

  let variantsVisible = false;
  let pickerVisible = false;

  let variants;
  let currentEmoji;
  let searchText;

  const dispatch = createEventDispatcher();

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

  function hidePicker(event) {
    pickerVisible = false;
    popper.destroy();
  }

  async function togglePicker() {
    pickerVisible = !pickerVisible;

    if (pickerVisible) {
      await tick();
      popper = new Popper(triggerButtonEl, pickerEl, {
        placement: 'right'
      });
    } else {
      popper.destroy();
    }
  }

  function onKeyDown(event) {
    if (event.key === 'Escape') {
      hidePicker();
    }
  }

  function showEmojiDetails(event) {
    currentEmoji = event.detail;
  }

  function onEmojiClick(event) {
    if (event.detail.variants) {
      variants = event.detail.variants;
      variantsVisible = true;
    } else {
      dispatch('emoji', event.detail.emoji);
      hidePicker();
    }
  }

  function onVariantClick(event) {
    dispatch('emoji', event.detail.emoji);
    hideVariants();
    hidePicker();
  }

  function hideVariants() {
    // We have to defer the removal of the variants popup.
    // Otherwise, it gets removed before the click event on the body
    // happens, and the target will have a `null` parent, which
    // means it will not be excluded and the clickoutside event will fire.
    setTimeout(() => {
      variantsVisible = false;
    });
  }
</script>

<style>
  .svelte-emoji-picker {
    background: #FFFFFF;
    border: 1px solid #CCCCCC;
    border-radius: 5px;
    width: 22rem;
    height: 21rem;
    margin: 0 0.5em;
    box-shadow: 0px 0px 3px 1px #CCCCCC;
  }

  .svelte-emoji-picker__trigger {
    cursor: pointer;
  }

  .svelte-emoji-picker__emoji-tabs {
    padding: 0.25em;
    height: 15rem;
  }

  :global(.svelte-emoji-picker__emoji-tabs .svelte-tabs ul.svelte-tabs__tab-list) {
    display: flex;
  }

  :global(.svelte-emoji-picker__emoji-tabs .svelte-tabs li.svelte-tabs__tab) {
    flex-grow: 1;
  }
</style>

<svelte:body on:keydown={onKeyDown} />

<button class="svelte-emoji-picker__trigger" bind:this={triggerButtonEl} on:click={togglePicker}>
  <Icon icon={smileIcon} />
</button>

{#if pickerVisible}
  <ClickOutside on:clickoutside={hidePicker} exclude={[triggerButtonEl]}>
    <div class="svelte-emoji-picker" bind:this={pickerEl} on:keydown={onKeyDown}>
      <EmojiSearch bind:searchText={searchText} />
      {#if searchText}
        <EmojiSearchResults searchText={searchText} on:emojihover={showEmojiDetails} on:emojiclick={onEmojiClick} />
      {:else}
        <div class="svelte-emoji-picker__emoji-tabs">
          <Tabs> 
            <TabList>
              {#each categoryOrder as category}
                <Tab><Icon icon={categoryIcons[category]} /></Tab>
              {/each}
            </TabList>

            {#each categoryOrder as category}
              <TabPanel>
                <EmojiList name={category} emojis={emojiCategories[category]} on:emojihover={showEmojiDetails} on:emojiclick={onEmojiClick} />
              </TabPanel>
            {/each}
          </Tabs>
        </div>

        {#if variantsVisible}
          <VariantPopup variants={variants} on:emojiclick={onVariantClick} on:close={hideVariants} />
        {/if}
      {/if}

      <EmojiDetail emoji={currentEmoji} />
    </div>
  </ClickOutside>
{/if}
