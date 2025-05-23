<script lang="ts">
import Main from '$lib/main.svelte';
import Obstacle from '$lib/obstacle.svelte';
import Coin from '$lib/coin.svelte';
	import { onMount } from 'svelte';
    let gameOver = false;
    let gameStart = false;
    let points: number = 0;
onMount(()=>
{
    window.addEventListener('gameOver',() => gameOver = true);
    window.addEventListener('addPoint',() => points++);
})
</script>
<div class="relative w-[500px] h-[600px] border-1 mx-auto bg-[url(/grafiki/background.png)]">
{#if gameStart}
<Obstacle />
<Obstacle />
<Obstacle />
<Obstacle />
<Coin />
<Main />
{#if gameOver}
<img src="/grafiki/buttonsAndGameOver/gameOverScreen.png" alt="game over" class="absolute left-1/3 top-1/3 w-[200px] h-[100px]">
{/if}
{/if}
</div>
<button onclick={() => gameStart=true} class="bg-blue-500 text-white p-2">Start game</button>
<button onclick={() => {window.location.reload()}} class="bg-blue-500 text-white p-2">Try again</button>
<p class="bg-blue-500 text-white p-2 inline">Points:{points}</p>