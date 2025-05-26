<script lang="ts">
	import { onMount } from 'svelte';
	let posX = $state(250);
	let posY = $state(500);
	let vel = $state(7);
	let moveLeft = false;
	let moveRight = false;
	let gameOver = false;
	let animationRight = false;
	const framesRight = ['/grafiki/animationRight/animationRight1.png','/grafiki/animationRight/animationRight2-4.png','/grafiki/animationRight/animationRight3.png','/grafiki/animationRight/animationRight2-4.png'];
	let indexRight = $state(0);
	const framesLeft = ['/grafiki/animationLeft/animationLeft1.png','/grafiki/animationLeft/animationLeft2-4.png','/grafiki/animationLeft/animationLeft3.png','/grafiki/animationLeft/animationLeft2-4.png'];
	let indexLeft = $state(0);
	function keyDown(event: KeyboardEvent) {
		if (!gameOver) {
			switch (event.key) {
				case 'd':
					if (posX < 436) {
						moveRight = true;
					}
					break;
				case 'a':
					if (posX > 0) {
						moveLeft = true;
					}
					break;
			}
		}
	}
	function keyUp(event: KeyboardEvent) {
		switch (event.key) {
			case 'd':
				moveRight = false;
				break;
			case 'a':
				moveLeft = false;
				break;
		}
	}
	function animateRight()
	{
		let handler = document.getElementById('playerImage') as HTMLImageElement;
		handler.src = framesRight[indexRight];
		if(indexRight <= 1)
	{
		indexRight++;
	}
	else
	{
		indexRight = 0;
	}
	}
	function animateLeft()
	{
		let handler = document.getElementById('playerImage') as HTMLImageElement;
		handler.src = framesLeft[indexLeft];
		if(indexLeft <= 1)
	{
		indexLeft++;
	}
	else
	{
		indexLeft = 0;
	}
	}
	function move() {
		if (moveRight && posX < 436) {
			posX += vel;
			animateRight();
		}
		if (moveLeft && posX > 0) {
			posX -= vel;
			animateLeft();
		}
		let x = posX;
		let y = posY;
		window.dispatchEvent(new CustomEvent('playerMove', { detail: { x, y } }));
		setTimeout(() => requestAnimationFrame(move),60);
	}
	onMount(() => {
		window.addEventListener('keydown', keyDown);
		window.addEventListener('keyup', keyUp);
		window.addEventListener('gameOver', () => (gameOver = true));
		move();
	});
</script>

<div class="absolute h-16 w-16 " style="left: {posX}px; top: {posY}px" id="main"><img src="/grafiki/New Piskel.png" alt="player" id="playerImage"></div>
