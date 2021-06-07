<script>
	import { onMount } from 'svelte';
	import dayjs from 'dayjs';
	import customParseFormat from 'dayjs/plugin/customParseFormat';
	import weekOfYear from 'dayjs/plugin/weekOfYear';
	import preciseDiff from 'dayjs-precise-range';

	dayjs.extend(customParseFormat);
	dayjs.extend(weekOfYear);
	dayjs.extend(preciseDiff)

	let characters = ['🥳', '🎉', '✨'];

	let confetti = new Array(50).fill()
		.map((_, i) => {
			return {
				character: characters[i % characters.length],
				x: Math.random() * 100,
				y: -20 - Math.random() * 100,
				r: 0.1 + Math.random() * 1
			};
		})
		.sort((a, b) => a.r - b.r);

	onMount(() => {
		let frame;

		function loop() {
			frame = requestAnimationFrame(loop);

			confetti = confetti.map(emoji => {
				emoji.y += 0.7 * emoji.r;
				if (emoji.y > 120) emoji.y = -20;
				return emoji;
			});
		}

		loop();

		return () => cancelAnimationFrame(frame);
	});

	let christmas = dayjs('2020-24-12', 'YYYY-DD-MM');
	let celebrate = false;

	const calcDiff = (json = false) => {
		let time = dayjs.preciseDiff(christmas, dayjs(), true);
		if (json) {
			return time;
		}
		let timeStr = `
			${time.years !== 0 ? time.years + ' år<br>' : '' }
			${time.months !== 0 ? (time.months == 1 ? time.months + ' månad<br>' : time.months + ' månader<br>') : ''}
			${time.days >= 7 ? (Math.floor(time.days / 7) == 1 ? Math.floor(time.days / 7) + ' vecka<br>' : Math.floor(time.days / 7) + ' veckor<br>') : ''}
			${time.days % 7 !== 0 ? (Math.floor(time.days % 1) === 1 ? Math.floor(time.days % 7) + ' dag<br>' : Math.floor(time.days % 7) + ' dagar<br>') : ''}
		`;
		return timeStr.substring(0, timeStr.length - 7);
	}

	let date = calcDiff(true)

	if (date.days === 0) {
		celebrate = true;
	}

</script>

<p class="week">Vecka {dayjs().week()}</p>

<p class="date">{@html calcDiff()}</p>

{#if celebrate}
	{#each confetti as c}
		<span style="left: {c.x}%; top: {c.y}%; transform: scale({c.r})">{c.character}</span>
	{/each}
{/if}

<style>
	* {
		padding: 0;
		margin: 0;
		box-sizing: border-box;
	}

	:global(body) {
		overflow: hidden;
		background-color:#393E41;
		color: #fff;
		height: 100vh;
		display: grid;
		place-content: center;
		font-family: 'Nunito', sans-serif;
	}

	span {
		position: absolute;
		font-size: 5vw;
	}

	.week {
		position: absolute;
		top: 0;
		left: 0;
		padding: .7em;
		font-size: 1.25em;
	}
	.date {
		font-size: 3em;
		text-align: center;
	}
<<<<<<< HEAD
</style>
=======
</style>
>>>>>>> gh-pages
