<script lang="ts">
	import { onMount } from 'svelte';
	const {
		min,
		max
	}:
	{
		min: number;
		max: number;
	} = $props();
	let posX = $state(0);
	getX();
	let posY = $state(100);
	let vel = Math.random() * 4;
	newVel()
	let isVisible = $state(true);
	let pPosX = $state(250);
	let pPosY = $state(500);
	let gameOver = false;
	function getX()
	{
		posX = Math.round(Math.random() * 436);
		while(posX < min || posX > max)
	{
		posX = Math.round(Math.random() * 436);
	}
	}
	function newVel()
	{
		vel = Math.round(Math.random() *4);
		while (vel <= 1) {
		vel = Math.round(Math.random() *4);
	}
	}
	function endGame() {
		window.dispatchEvent(new CustomEvent('gameOver'));
		gameOver = true;
	}
	function moveP(event: { detail: { x: number; y: number; }; }): void {
		pPosX = event.detail.x;
		pPosY = event.detail.y;
	}
	function restart() {
		posY = 100;
		getX();
		newVel();
		isVisible = true;
		move();
	}
	function move() {
		if (posY < 500 && !gameOver) {
			posY += vel;
			if (pPosX - 50 <= posX && pPosX + 50 >= posX) {
				if (pPosY - 50 <= posY && pPosY + 50 >= posY) {
					endGame();
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
	<div class="absolute h-16 w-16" style="left: {posX}px; top: {posY}px" id="main"><img src="/grafiki/kula.png" alt="rock"></div>
{/if}
