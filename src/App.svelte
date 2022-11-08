<script>
    import { afterUpdate } from "svelte";


	let numberOfCashDesk = 1;

	$: if (numberOfCashDesk < 1) {
		numberOfCashDesk = 1
	}

	let timePeriod = 1;

	$: if (timePeriod < 1) {
		timePeriod = 1
	}

	let numberOfPeople = 5;

	$: if (numberOfPeople < 0) {
		numberOfPeople = 0
	}

	let minTimePeriodForCashDesk = 3;

	$: if (minTimePeriodForCashDesk < 1) {
		minTimePeriodForCashDesk = 1
	}

	let maxTimePeriodForCashDesk = 10;

	$: if (maxTimePeriodForCashDesk < 1) {
		maxTimePeriodForCashDesk = 1
	}

	const getRandomIntFromInterval = (min, max) => Math.floor(Math.random() * (max - min + 1) + min)


	let cashDesksInfo = []

	const startEmulation = () => {
		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod)
		let randomNumberOfPeople = getRandomIntFromInterval(0, numberOfPeople)

		cashDesksInfo = []

		for (let index = 1; index <= numberOfCashDesk; index++) {
			
			let timePeriodForCashDesk = getRandomIntFromInterval(minTimePeriodForCashDesk, maxTimePeriodForCashDesk)
			cashDesksInfo.push({
				time: timePeriodForCashDesk,
				numberOfPeopleAtOneCashDesk: 0
			})
			
		}

		console.log(cashDesksInfo)
	}


	afterUpdate(() => {
		startEmulation()
	})


</script>

<main>
	<div class="container">
		<label for="input-numberOfCashDesk">Количество касс:</label>
		<input bind:value={numberOfCashDesk} id="input-numberOfCashDesk" type="number" />

		<label for="input-timePeriod">Период времени в секундах:</label>
		<input bind:value={timePeriod} id="input-timePeriod" type="number" />

		<label for="input-numberOfPeople">Количество людей:</label>
		<input bind:value={numberOfPeople} id="input-numberOfPeople" type="number" />

		<label for="input-minTimePeriodForCashDesk">Минимальное время обслуживания кассы:</label>
		<input bind:value={minTimePeriodForCashDesk} id="input-minTimePeriodForCashDesk" type="number" />

		<label for="input-maxTimePeriodForCashDesk">Максимальное время обслуживания кассы:</label>
		<input bind:value={maxTimePeriodForCashDesk} id="input-maxTimePeriodForCashDesk" type="number" />

		
		<div class="cashDesks">
			{#each cashDesksInfo as cashDesk}
				<div class="cashDesk">{cashDesk.time}</div>
			{/each}
		</div>
		
		
	</div>
</main>

<style>
	.cashDesk{
		display: inline-block;
		width: 50px;
		height: 200px;
		background: green;
		margin-top: 50px;
		margin-left: 10px;
		color: white;
		text-align: center;
	}
</style>