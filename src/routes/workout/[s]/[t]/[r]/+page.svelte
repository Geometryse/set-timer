<script lang="ts">
	import { page } from '$app/stores';
	import { now } from 'svelte/internal';
	import { Stopwatch } from 'ts-stopwatch';
	enum Status {
		WORKOUT,
		REST
	}
	function formatTime(inputTime: number) {
		const minutes = Math.floor(inputTime / 60);
		const seconds = Math.floor(inputTime % 60);
		return [minutes, seconds].join(':').replace(/\b(\d)\b/g, '0$1');
	}
	// variables from URL
	const sets = $page.params.s;
	const timePerSet = parseInt($page.params.t);
	const restTime = parseInt($page.params.r);
	$: timeToUse = status === Status.WORKOUT ? timePerSet : restTime;
	$: formattedTime = formatTime(timeToUse - millis / 1000);
	$: formattedTimeElapsed = formatTime(initialMillis / 1000);
	// Stopwatch milliseconds
	let millis = 0;
	let initialMillis = 0;
	setInterval(() => {
		initialMillis = Date.now() - initialStartTime;
		if (timeRunning) {
			const newMillis = Date.now() - startTime;
			if (timeToUse - newMillis / 1000 < 0) {
				// Times up, new activity
				startTime = Date.now();
				if (status === Status.WORKOUT) status = Status.REST;
				else {
					status = Status.WORKOUT;
					completedSets += 1;
				}
				millis = 0;
			} else millis = newMillis;
		}
	}, 100);
	const initialStartTime = Date.now();
	let startTime: number = Date.now();
	let pauseTime: number | null;
	let timeRunning = true;
	let status: Status = Status.WORKOUT;
	let completedSets = 0;
	function handleToggleTimer() {
		// If Resuming
		if (!timeRunning && pauseTime) startTime += Date.now() - pauseTime;
		// Else Pausing
		else pauseTime = Date.now();
		timeRunning = !timeRunning;
	}
</script>

<div
	class="{timeRunning
		? status === Status.WORKOUT
			? 'bg-blue-500/20'
			: 'bg-green-500/20'
		: 'bg-transparent'} transition-colors h-screen"
>
	<div class="flex flex-col py-20 items-center">
		<p class="uppercase text-xl text-gray-300">set {completedSets + 1}/{sets}</p>
		<h1 class="text-4xl font-medium">
			{status === Status.WORKOUT ? 'WORK OUT' : 'REST'}
		</h1>
		<h1 class="text-gray-300 font-medium text-sm">time elapsed: {formattedTimeElapsed}</h1>
		<h1 class="text-8xl font-semibold my-10">{formattedTime}</h1>
		<div
			class="h-40 w-40 rounded-full flex items-center justify-center border select-none mt-6 {timeRunning
				? 'bg-red-600/10 border-red-500 hover:bg-red-600/30 active:bg-red-600/20'
				: 'bg-green-600/10 border-green-500 hover:bg-green-600/30 active:bg-green-600/20'} cursor-pointer transition"
			on:click={handleToggleTimer}
		>
			<h1 class="text-2xl font-bold text-center {timeRunning ? 'text-red-500' : 'text-green-500'} ">
				{timeRunning ? 'PAUSE' : 'RESUME'}
			</h1>
		</div>
	</div>
</div>
