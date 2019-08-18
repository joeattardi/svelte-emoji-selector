<script>
	import EmojiPicker from 'svelte-emoji-picker';

	let textContent = '';

	function onEmoji(event) {
		textContent += event.detail;
	}
</script>

<style>
</style>

<h1>Svelte Emoji Picker</h1>

<input bind:value={textContent} type="text">
<EmojiPicker on:emojiclick={onEmoji} />
