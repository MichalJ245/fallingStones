<script lang="ts">
import Main from '$lib/main.svelte';
import Obstacle from '$lib/obstacle.svelte';
import Coin from '$lib/coin.svelte';
	import { onMount } from 'svelte';
    let gameOver = $state(false);
    let gameStart = $state(false);
    let points: number = 0;
    let firstDigit: number = $state(0);
    let secondDigit: number = $state(0);
    function calculatePoints()
    {
        points++;
        console.log('aa')
        let pointString = points.toString();
        firstDigit = parseInt(pointString.charAt(pointString.length-1));
        if(points >= 10)
    {
        secondDigit = parseInt(pointString.charAt(0));
    }
    }
onMount(()=>
{
    window.addEventListener('gameOver',() => gameOver = true);
    window.addEventListener('addPoint',() => calculatePoints());
})
</script>
<div class="relative w-[500px] h-[600px] border-1 mx-auto bg-[url(/grafiki/background.png)]">
{#if gameStart}
<Obstacle min={0} max={125}/>
<Obstacle min ={125} max={250}/>
<Obstacle  min={250} max={375}/>
<Obstacle min={375} max={500}/>
<Coin />
<Main />
{#if gameOver}
<img src="/grafiki/buttonsAndGameOver/gameOverScreen.png" alt="game over" class="absolute left-1/3 top-1/3 w-[200px] h-[100px]">
{/if}
{/if}
</div>
<div class="flex justify-center">
<button onclick={() => gameStart=true}><img src="/grafiki/buttonsAndGameOver/start-button.png" alt="start"></button>
<button onclick={() => {window.location.reload()}}><img src="/grafiki/buttonsAndGameOver/try-again-button.png" alt="start"></button>
<p class="bg-[url(/grafiki/buttonsAndGameOver/points-screen.png)] relative w-25 h-12">
    {#if secondDigit > 0}
    <img src="/grafiki/Punkty/{secondDigit}-point.png" alt="number" class="absolute" style="top:6px; left: 75px;">
    {/if}
    <img src="/grafiki/Punkty/{firstDigit}-point.png" alt="number" class="absolute" style="top:6px; left: 100px;"></p>
</div>