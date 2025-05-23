<script lang="ts">
	import { onMount } from 'svelte';
	let posX = $state(Math.round(Math.random() * 436));
	let posY = $state(100);
	let vel = Math.random() * 3;
	newVel()
	console.log(vel);
	let isVisible = $state(true);
	let pPosX = $state(250);
	let pPosY = $state(500);
	let gameOver = false;
	function newVel()
	{
		vel = Math.round(Math.random() *3);
		while (vel <= 0) {
		vel = Math.round(Math.random() *3);
		console.log(vel);
	}
	}
	function moveP(event: { detail: { x: number; y: number; }; }): void {
		pPosX = event.detail.x;
		pPosY = event.detail.y;
	}
	function restart() {
        
		posY = 100;
		posX = Math.round(Math.random() * 500);
		newVel();
		isVisible = true;
		move();
	}
	function move() {
		if (posY < 500 && !gameOver) {
			posY += vel;
			if (pPosX - 64 <= posX && pPosX + 64 >= posX) {
				if (pPosY - 64 <= posY && pPosY + 64 >= posY) {
                    restart();
					window.dispatchEvent(new CustomEvent('addPoint'));
                    
				}
			}
			requestAnimationFrame(move);
		} else {
			if (gameOver) {
				isVisible = false;
			} else {
				isVisible = false;
				restart();
			}
		}
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
	<div class="absolute h-16 w-16" style="left: {posX}px; top: {posY}px"><img src="/grafiki/animationCoin/coin1.png" alt="rock" id="coin"></div>
{/if}
