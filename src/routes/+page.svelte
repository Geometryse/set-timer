<script lang="ts">
	const setPossibilities = [1, 2, 3, 4, 5] as const;
	let sets: typeof setPossibilities[number] = 3;
	let timePerSet = 35;
	let restTime = 90;
	$: redirectUrl = `workout/${sets}/${timePerSet}/${restTime}`;
</script>

<div class="flex w-full h-screen items-center flex-col pt-20">
	<h1 class="text-3xl font-semibold mb-10">Welcome to Set Timer</h1>
	<div class="space-y-4">
		<div class="flex items-center space-x-3 text-lg">
			<p>Target sets</p>
			<div class="dropdown">
				<!-- svelte-ignore a11y-label-has-associated-control -->
				<label tabindex="0" class="btn m-1 bg-[#2A303C]">{sets}</label>
				<ul tabindex="0" class="dropdown-content menu p-2 shadow bg-base-100 rounded-box">
					{#each setPossibilities as set}
						<!-- svelte-ignore a11y-missing-attribute -->
						<li><a>{set}</a></li>
					{/each}
				</ul>
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
