# svelte-emoji-selector

Svelte component for picking emojis ⚡️

![Screenshot](https://raw.githubusercontent.com/joeattardi/svelte-emoji-selector/master/screenshot.png)

## Demo
[https://joeattardi.github.io/svelte-emoji-selector/](https://joeattardi.github.io/svelte-emoji-selector/)

## Features
 - Emoji search
 - Skin tone variation selector
 - Recently used emojis

## Installation

    npm install --save svelte-emoji-selector

## Basic Usage

When the component is used, it will render a button with a smiley face on it. When that button is clicked, the emoji selector will be displayed.

When an emoji is clicked in the selector, it will emit an `emoji` event. The event detail will be the actual emoji character. From there you can do what you want with the emoji, such as inserting it into a text field.

Here is a simple example:

```html
<script>
  import EmojiSelector from 'svelte-emoji-selector';

  let textContent = '';

  function onEmoji(event) {
    textContent += event.detail;
  }
</script>

<input bind:value={textContent} type="text">
<EmojiSelector on:emoji={onEmoji} />
```

## Props

- `autoClose` (default: `true`): Whether the selector should automatically close after an emoji is hidden. If you want to allow the user to select multiple emojis without having to re-open the selector, set this to `false`. If this is `false`, the selector can be closed by clicking anywhere else in the page, clicking the trigger button again, or pressing the Escape key.

- `maxRecents` (default: `50`): The maximum number of emojis to save in the "recently used" tab.

## Events

- `emoji`: Fired whenever an emoji is clicked. The event detail is the emoji character itself.
