<script lang="ts">
	import { onMount } from 'svelte';

	let languages = ['English', 'Chinese', 'Spanish', 'French', 'German'];
	let selectedLanguage = 'English';
	let textareaElement;

	const adjustHeight = () => {
		textareaElement.style.height = 'auto';
		textareaElement.style.height = `${textareaElement.scrollHeight}px`;
	};

	// Adjust height when the component mounts
	onMount(() => {
		adjustHeight();
	});
	const selectLanguage = (language: string) => {
		selectedLanguage = language;
	};

	let sourceText = '';
	let translatedText = '';

	let detectedLanguage = '';
	let targetLanguage = 'Spanish';
	let recentLanguages: string[] = [];

	const detectLanguage = async () => {
		// Here you would call the API to detect the language.
		// For now, we'll just set it to English
		detectedLanguage = 'English';
	};

	const translateText = async () => {
		// ...previous script code...
		translatedText = sourceText.split('').reverse().join('');

		// Add the selected target language to recent languages
		if (!recentLanguages.includes(selectedLanguage)) {
			recentLanguages.unshift(selectedLanguage);
			if (recentLanguages.length > 5) {
				recentLanguages.pop();
			}
		}
	};

	const clearText = () => {
		sourceText = '';
		translatedText = '';
	};

	const copyTranslation = () => {
		navigator.clipboard.writeText(translatedText);
	};
</script>

<header class="flex items-center justify-between bg-blue-500 p-6 text-white">
	<h1 class="text-2xl">AI Assisting Translate</h1>
</header>

<div class="p-6">
	<div class="flex">
		<div class="relative inline-flex">
			<svg
				class="pointer-events-none absolute right-0 top-0 m-4 h-2 w-2"
				xmlns="http://www.w3.org/2000/svg"
				viewBox="0 0 412 232"
				><path
					d="M206 171.144L42.678 7.822c-9.763-9.763-25.592-9.763-35.355 0-9.763 9.763-9.763 25.592 0 35.355l181 181c9.763 9.763 25.592 9.763 35.355 0l181-181c9.762-9.763 9.762-25.592 0-35.355-9.763-9.763-25.592-9.763-35.355 0L206 171.144z"
					fill="#648299"
					fill-rule="nonzero"
				/></svg
			>
			<select
				class="h-10 appearance-none rounded-full border border-gray-300 bg-white pl-5 pr-10 text-gray-600 hover:border-gray-400 focus:outline-none"
				bind:value={selectedLanguage}
				on:change={(e) => selectLanguage(e.target.value)}
			>
				{#each languages as language (language)}
					<option>{language}</option>
				{/each}
			</select>
		</div>
	</div>
	<div class="grid grid-cols-2 gap-4">
		<div class="flex items-start rounded border border-gray-300 p-4">
			<textarea
				class="w-full resize-none focus:outline-none focus:ring-0"
				bind:value={sourceText}
				bind:this={textareaElement}
				on:input={adjustHeight}
			/>
			{#if sourceText}
				<button
					class="ml-2 flex h-8 w-8 items-center justify-center rounded-full bg-transparent text-gray-800 hover:bg-gray-200"
					on:click={clearText}>X</button
				>
			{/if}
		</div>

		<div class="rounded border border-gray-300 p-4">
			<p class="break-all">{translatedText}</p>
		</div>
	</div>
</div>
