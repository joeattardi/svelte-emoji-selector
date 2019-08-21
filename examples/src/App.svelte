<script>
	import EmojiSelector from 'svelte-emoji-selector';

	let textContent = '';

	function onEmoji(event) {
		textContent += event.detail;
	}
</script>

<style>
</style>

<h1>Svelte Emoji Selector</h1>

<input bind:value={textContent} type="text">
<EmojiSelector on:emoji={onEmoji} />
