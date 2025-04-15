<script lang="ts">
	import type { Joke } from '$types';
	import { writable } from 'svelte/store';
	import { loadChuckFact } from '$lib/utils/jokeHelper';

	const chuckFactJoke = writable<Joke | null>(null);

	async function fetchChuckFact() {
		try {
			const data = await loadChuckFact();
			chuckFactJoke.set(data);
		} catch (error) {
			console.error('Error fetching Chuck Norris fact:', error);
		}
	}

	function refreshJoke() {
		fetchChuckFact();
	}

	fetchChuckFact();
</script>

<h1>Chuck Norris fact of the day</h1>
<button on:click={refreshJoke}>Refresh</button>

{#if $chuckFactJoke}
    <p>{JSON.stringify($chuckFactJoke)}</p>
{:else}
    <p>Loading...</p>
{/if}

