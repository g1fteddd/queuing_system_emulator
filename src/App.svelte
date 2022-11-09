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

	let minTimePeriodForCashDesk = 5;

	$: if (minTimePeriodForCashDesk < 1) {
		minTimePeriodForCashDesk = 1
	}

	let maxTimePeriodForCashDesk = 5;

	$: if (maxTimePeriodForCashDesk < 1) {
		maxTimePeriodForCashDesk = 1
	}

	const getRandomIntFromInterval = (min, max) => Math.floor(Math.random() * (max - min + 1) + min)

	const getMinValueInArray = (array) => {
		let min = array[0].numberOfPeopleAtOneCashDesk
		let position = 0
		
		for (let index = 1; index < array.length; index++) {
			if (array[index].numberOfPeopleAtOneCashDesk < min) {
				min = array[index].numberOfPeopleAtOneCashDesk
				position = index
			}
		}

		return position
	}

	let isPaused = false

	const pause = () => {
		isPaused = !isPaused
	}

	let cashDesksInfo = []

	const startEmulation = (randomTimePeriod) => {
		
		

		setInterval(() => {
			if (!isPaused) {
				let randomNumberOfPeople = getRandomIntFromInterval(1, numberOfPeople) // вместо 1 поставить 0
				for (let index = 0; index < randomNumberOfPeople; index++) {
					let minCashDeskIndex = getMinValueInArray(cashDesksInfo)
					cashDesksInfo[minCashDeskIndex].numberOfPeopleAtOneCashDesk++
				}
			}
		}, randomTimePeriod*1000);
		

		
		for (let index = 0; index < cashDesksInfo.length; index++) {
				let timePeriodForCashDesk = getRandomIntFromInterval(minTimePeriodForCashDesk, maxTimePeriodForCashDesk)
				cashDesksInfo[index].time = timePeriodForCashDesk 
				
				setInterval(() => {
					console.log(cashDesksInfo[index].time)
					if (!isPaused) {
						cashDesksInfo[index].numberOfPeopleAtOneCashDesk--
						if (cashDesksInfo[index].numberOfPeopleAtOneCashDesk < 0) {
							cashDesksInfo[index].numberOfPeopleAtOneCashDesk = 0
						}
						cashDesksInfo[index].time = getRandomIntFromInterval(minTimePeriodForCashDesk, maxTimePeriodForCashDesk)
					}
				}, cashDesksInfo[index].time*1000)
			}


		
	}


	

	const initEmulation = () => {
		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod)
		

		cashDesksInfo = []

		for (let index = 0; index < numberOfCashDesk; index++) {
			cashDesksInfo.push({
				time: 0,
				numberOfPeopleAtOneCashDesk: 0
			})
			
		}

		console.log(cashDesksInfo)


		startEmulation(randomTimePeriod)
		

		
	}


	


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

		<button on:click={initEmulation}>Запустить процесс</button>
		<button on:click={pause}>Пауза</button>

		
		<div class="cashDesks">
			{#each cashDesksInfo as cashDesk}
				<div class="cashDesk">
					<p>{cashDesk.time}</p>
					<p>{cashDesk.numberOfPeopleAtOneCashDesk}</p>
				</div>
			{/each}
		</div>
		
		
	</div>
</main>

<style>
	input {
		display: block;
	}

	.cashDesk{
		display: inline-block;
		width: 50px;
		height: 200px;
		background: black;
		margin-top: 50px;
		margin-left: 10px;
		color: white;
		text-align: center;
	}
</style>