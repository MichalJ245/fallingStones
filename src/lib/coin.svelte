<script lang="ts">
	import { onMount } from 'svelte';
	let posX = $state(Math.round(Math.random() * 400));
	let posY = $state(100);
	let vel = Math.random() * 4;
	newVel()
	let isVisible = $state(true);
	let pPosX = $state(250);
	let pPosY = $state(500);
	let gameOver = false;
	const frames = ['/grafiki/animationCoin/coin1.png','/grafiki/animationCoin/coin2.png','/grafiki/animationCoin/coin3.png','/grafiki/animationCoin/coin4.png'];
	let index = $state(0);
	let doAnimate = $state(0);
	function animate()
	{
		let handler = document.getElementById('coin') as HTMLImageElement;
		handler.src = frames[index];
		if(index <= 2)
	{
		index++;
	}
	else
	{
		index = 0;
	}
	}
	function newVel()
	{
		vel = Math.round(Math.random() *4);
		while (vel <= 1) {
		vel = Math.round(Math.random() *4);
	}
	}
	function moveP(event: { detail: { x: number; y: number; }; }): void {
		pPosX = event.detail.x;
		pPosY = event.detail.y;
	}
	function restart() {
		posY = 100;
		posX = Math.round(Math.random() * 400);
		newVel();
		isVisible = true;
		move();
	}
	function move() {
		if (posY < 500 && !gameOver) {
			posY += vel;
			if (pPosX - 50 <= posX && pPosX + 50 >= posX) {
				if (pPosY - 50 <= posY && pPosY + 50 >= posY) {
					window.dispatchEvent(new CustomEvent('addPoint'));
					restart();
				}
				else
				{
					requestAnimationFrame(move);
				}
			}
			else
			{
			requestAnimationFrame(move);
			}
		} else {
			if (gameOver) {
				isVisible = false;
			} else {
				isVisible = false;
				restart();
			}
		}
		if(doAnimate %3== 0)
		{
			animate();
		}
		doAnimate++;
	}
	onMount(() => {
		window.addEventListener('playerMove', (event) => moveP(event as CustomEvent<{ x: number; y: number }>));
		if (!gameOver) {
			move();
		}
		window.addEventListener('gameOver',() => gameOver = true);
	});
</script>

{#if isVisible}
	<div class="absolute h-16 w-16" style="left: {posX}px; top: {posY}px" id="main"><img src="/grafiki/animationCoin/coin1.png" alt="coin" id="coin"></div>
{/if}
