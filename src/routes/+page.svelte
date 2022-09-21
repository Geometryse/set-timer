<script lang="ts">
	const setPossibilities = [
		1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20
	] as const;
	let sets: number;
	let timePerSet: number;
	let restTime: number;
	$: redirectUrl = `workout/${sets}/${timePerSet}/${restTime}`;
</script>

<div class="flex w-full h-screen items-center flex-col pt-20">
	<h1 class="text-3xl font-semibold mb-10">Welcome to Set Timer</h1>
	<div class="space-y-4">
		<div class="flex items-center space-x-3 text-lg">
			<p>Target sets</p>
			<div class="dropdown">
				<!-- svelte-ignore a11y-label-has-associated-control -->
				<input type="number" bind:value={sets} class="input w-20 max-w-xs" />
			</div>
		</div>
		<div class="flex items-center space-x-3 text-lg">
			<p>Time per set (sec.)</p>
			<input type="number" bind:value={timePerSet} class="input w-20 max-w-xs" />
		</div>
		<div class="flex items-center space-x-3 text-lg">
			<p>Rest time (sec.)</p>

			<input type="number" bind:value={restTime} class="input w-20 max-w-xs" />
		</div>
	</div>
	<div
		class="h-40 w-40 bg-green-600/10 rounded-full flex items-center justify-center border mt-32 border-green-500 hover:bg-green-600/30 active:bg-green-600/20 cursor-pointer transition"
		on:click={() => (location.href = redirectUrl)}
	>
		<h1 class="text-2xl font-bold text-center text-green-500">START<br /> WORKOUT</h1>
	</div>
</div>
