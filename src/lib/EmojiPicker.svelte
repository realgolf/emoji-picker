<script lang="ts">
  import { afterUpdate, onMount } from 'svelte';
  import { Emoji, emojis } from '../data/emoji';

  let filteredEmojis: Emoji[] = emojis;
  let searchTerm = '';

  const filterEmojis = () => {
    filteredEmojis = emojis.filter(emoji =>
      emoji.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  };

  const selectEmoji = (emoji: Emoji) => {
    const selectedEmoji = emoji.emoji;
    console.log("Selected Emoji:", selectedEmoji);
    // You can use selectedEmoji however you need in your application
  };

  onMount(() => {
    filterEmojis();
  });

  afterUpdate(() => {
    filterEmojis();
  });
</script>

<style>
.emoji-picker {
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
}
</style>

<div class="emoji-picker">
<input type="text" bind:value={searchTerm} placeholder="Search emojis..." />

<div>
  {#if filteredEmojis.length > 0}
    {#each filteredEmojis as emoji (emoji.key)}
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <span
        class="emoji"
        on:click={() => selectEmoji(emoji)}
        title={emoji.name}
      >{emoji.emoji}</span>
      {#if emoji.variants}
        {#each Object.values(emoji.variants) as variant (variant.key)}
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <span
            class="emoji"
            on:click={() => selectEmoji(variant)}
            title={variant.name}
          >{variant.emoji}</span>
        {/each}
      {/if}
    {/each}
  {:else}
    <p>No emojis found.</p>
  {/if}
</div>
</div>
