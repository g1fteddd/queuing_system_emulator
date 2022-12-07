<script>
	import { afterUpdate } from "svelte";
    import { check_outros } from "svelte/internal";

	

	let numberOfCashDesk = 1;

	$: if (numberOfCashDesk < 1) {
		numberOfCashDesk = 1;
	}

	let timePeriod = 1;

	$: if (timePeriod < 1) {
		timePeriod = 1;
	}

	let numberOfPeople = 5;

	$: if (numberOfPeople < 0) {
		numberOfPeople = 0;
	}

	let minTimePeriodForCashDesk = [5];

	// $: if (minTimePeriodForCashDesk < 1) {
	// 	minTimePeriodForCashDesk = 1;
	// }

	let maxTimePeriodForCashDesk = [5];

	// $: if (maxTimePeriodForCashDesk < 1) {
	// 	maxTimePeriodForCashDesk = 1;
	// }

	const getRandomIntFromInterval = (min, max) =>
		Math.floor(Math.random() * (max - min + 1) + min);

	const getMinValueInArray = (array) => {
		let min = array[0].numberOfPeopleAtOneCashDesk;
		let position = 0;

		for (let index = 1; index < array.length; index++) {
			if (array[index].numberOfPeopleAtOneCashDesk < min) {
				min = array[index].numberOfPeopleAtOneCashDesk;
				position = index;
			}
		}

		return position;
	};

	const getMaxValueInArray = (array) => {
		let max = array[0].numberOfPeopleAtOneCashDesk;
		let position = 0;

		for (let index = 1; index < array.length; index++) {
			if (array[index].numberOfPeopleAtOneCashDesk > max) {
				max = array[index].numberOfPeopleAtOneCashDesk;
				position = index;
			}
		}

		return position;
	};

	let isPaused = false;

	const pause = () => {
		isPaused = !isPaused;
	};












	let cashDesksInfo = [];
	let a;
	let b;
	let arrayB = []

	const stopIntervals = () => {
		clearInterval(a);
		for (let index = 0; index < arrayB.length; index++) {
			clearInterval(arrayB[index])
			
		}
	};

	// const addNewCashDesk = (c) => {
	// 	stopIntervals();

	// 	console.log("isChangeData : true");
	// 	let timePeriodForCashDesk = getRandomIntFromInterval(
	// 		minTimePeriodForCashDesk,
	// 		maxTimePeriodForCashDesk
	// 	);
	// 	cashDesksInfo.push({
	// 		time: timePeriodForCashDesk,
	// 		numberOfPeopleAtOneCashDesk: 0,
	// 		avg: 0,
	// 		acceptPeople: [],
	// 	});
	// 	cashDesksInfo = cashDesksInfo;

	// 	let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);
	// 	startEmulation(randomTimePeriod);
	// };

	// $: if (numberOfCashDesk !== 1) {
	// 	addNewCashDesk(numberOfCashDesk);
	// }

	const clickMinusNumberOfCashDesk = () => {
		stopIntervals();
		cashDesksInfo.pop()
		minTimePeriodForCashDesk.pop()
		maxTimePeriodForCashDesk.pop()

		cashDesksInfo = cashDesksInfo;
		minTimePeriodForCashDesk = minTimePeriodForCashDesk
		maxTimePeriodForCashDesk = maxTimePeriodForCashDesk
		

		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);

		startEmulation(randomTimePeriod);
	};

	const clickPlusNumberOfCashDesk = () => {

		stopIntervals();
		minTimePeriodForCashDesk = [...minTimePeriodForCashDesk, 5]
		maxTimePeriodForCashDesk = [...maxTimePeriodForCashDesk, 5]
		let timePeriodForCashDesk = getRandomIntFromInterval(
			minTimePeriodForCashDesk[minTimePeriodForCashDesk.length - 1],
			maxTimePeriodForCashDesk[maxTimePeriodForCashDesk.length - 1]
		);
		cashDesksInfo.push({
			time: timePeriodForCashDesk,
			numberOfPeopleAtOneCashDesk: 0,
			avg: 0,
			acceptPeople: [],
		});
		
		cashDesksInfo = cashDesksInfo;

		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);

		startEmulation(randomTimePeriod);

	};



	const clickMinusTimePeriod = () => {
		stopIntervals();
		timePeriod--
		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);

		startEmulation(randomTimePeriod);
	}



	const clickPlusTimePeriod = () => {
		stopIntervals();
		timePeriod++
		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);

		startEmulation(randomTimePeriod);
	}


	const clickMinusNumberOfPeople = () => {
		stopIntervals();
		numberOfPeople--
		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);

		startEmulation(randomTimePeriod);
	}

	const clickPlusNumberOfPeople = () => {
		stopIntervals();
		numberOfPeople++
		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);

		startEmulation(randomTimePeriod);
	}



	const clickPlusMinTimePeriodForCashDesk = (index) => {
		stopIntervals();

		minTimePeriodForCashDesk[index]++

		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);
		startEmulation(randomTimePeriod);
	}

	const clickMinusMinTimePeriodForCashDesk = (index) => {
		stopIntervals();

		minTimePeriodForCashDesk[index]--

		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);
		startEmulation(randomTimePeriod);
	}

	const clickMinusMaxTimePeriodForCashDesk = (index) => {
		stopIntervals();

		maxTimePeriodForCashDesk[index]--

		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);
		startEmulation(randomTimePeriod);
	}

	const clickPlusMaxTimePeriodForCashDesk = (index) => {
		stopIntervals();

		maxTimePeriodForCashDesk[index]++

		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);
		startEmulation(randomTimePeriod);
	}

	let cashDeskElements = []

	const startEmulation = (randomTimePeriod) => {
		a = setInterval(() => {
			if (!isPaused) {
				let randomNumberOfPeople = getRandomIntFromInterval(
					1,
					numberOfPeople
				); // вместо 1 поставить 0
				for (let index = 0; index < randomNumberOfPeople; index++) {
					let minCashDeskIndex = getMinValueInArray(cashDesksInfo);
					cashDesksInfo[minCashDeskIndex]
						.numberOfPeopleAtOneCashDesk++;
				}
			}
		}, randomTimePeriod * 1000);



		for (let index = 0; index < cashDesksInfo.length; index++) {
			let timePeriodForCashDesk = getRandomIntFromInterval(
				minTimePeriodForCashDesk[index],
				maxTimePeriodForCashDesk[index]
			);
			cashDesksInfo[index].time = timePeriodForCashDesk;

			arrayB.push(setInterval(() => {
				console.log(cashDesksInfo[index].time);
				if (!isPaused) {
					cashDesksInfo[index].numberOfPeopleAtOneCashDesk--;
					cashDesksInfo[index].acceptPeople.push(
						cashDesksInfo[index].time
					);
					cashDesksInfo[index].avg =
						cashDesksInfo[index].acceptPeople.reduce(
							(prevValue, currentValue) => {
								return prevValue + currentValue;
							},
							0
						) / cashDesksInfo[index].acceptPeople.length;
					if (cashDesksInfo[index].numberOfPeopleAtOneCashDesk < 0) {
						cashDesksInfo[index].numberOfPeopleAtOneCashDesk = 0;
					}
					// cashDesksInfo[index].time = getRandomIntFromInterval(
					// 	minTimePeriodForCashDesk,
					// 	maxTimePeriodForCashDesk
					// );
				}
			}, cashDesksInfo[index].time * 1000))
		}

		setInterval(() => {
			for (let index = 0; index < cashDesksInfo.length; index++) {
				cashDeskElements[index].style.background = "black"
			}

			let minCashDeskIndex = getMinValueInArray(cashDesksInfo);
			cashDeskElements[minCashDeskIndex].style.background = "green"

			let maxCashDeskIndex = getMaxValueInArray(cashDesksInfo);
			cashDeskElements[maxCashDeskIndex].style.background = "red"
		}, 500)
	};

	const initEmulation = () => {
		let randomTimePeriod = getRandomIntFromInterval(1, timePeriod);

		cashDesksInfo = [];

		for (let index = 0; index < numberOfCashDesk; index++) {
			cashDesksInfo.push({
				time: 0,
				numberOfPeopleAtOneCashDesk: 0,
				avg: 0,
				acceptPeople: []
			});
		}

		console.log(cashDesksInfo);

		startEmulation(randomTimePeriod);
	};

	
</script>

<main>
	<div class="container">
		<label for="input-numberOfCashDesk">Количество касс:</label>
		<input
			bind:value={numberOfCashDesk}
			id="input-numberOfCashDesk"
			type="number"
		/>
		<button on:click={clickMinusNumberOfCashDesk}>-</button>
		<button on:click={clickPlusNumberOfCashDesk}>+</button>

		<label for="input-timePeriod">Период времени в секундах:</label>
		<input bind:value={timePeriod} id="input-timePeriod" type="number" />

		<button on:click={clickMinusTimePeriod}>-</button>
		<button on:click={clickPlusTimePeriod}>+</button>

		<label for="input-numberOfPeople">Количество людей:</label>
		<input
			bind:value={numberOfPeople}
			id="input-numberOfPeople"
			type="number"
		/>

		<button on:click={clickMinusNumberOfPeople}>-</button>
		<button on:click={clickPlusNumberOfPeople}>+</button>

		<!-- <label for="input-minTimePeriodForCashDesk"
			>Минимальное время обслуживания кассы:</label
		>
		<input
			bind:value={minTimePeriodForCashDesk}
			id="input-minTimePeriodForCashDesk"
			type="number"
		/>

		<label for="input-maxTimePeriodForCashDesk"
			>Максимальное время обслуживания кассы:</label
		>
		<input
			bind:value={maxTimePeriodForCashDesk}
			id="input-maxTimePeriodForCashDesk"
			type="number"
		/> -->

		<button on:click={initEmulation}>Запустить процесс</button>
		<button on:click={pause}>Пауза</button>

		<div class="cashDesks">
			{#each cashDesksInfo as cashDesk, index}
				<div class="cashDesk" bind:this={cashDeskElements[index]}>
					<p>Время: {cashDesk.time}</p>
					<p>Люди: {cashDesk.numberOfPeopleAtOneCashDesk}</p>
					<p>Среднее: {Math.floor(cashDesk.avg * 100)/100}</p>
					<input
						bind:value={minTimePeriodForCashDesk[index]}
						type="number"
					/>
					<button on:click={() => clickMinusMinTimePeriodForCashDesk(index)}>-</button>
					<button on:click={() => clickPlusMinTimePeriodForCashDesk(index)}>+</button>
					<input
						bind:value={maxTimePeriodForCashDesk[index]}
						type="number"
					/>
					<button on:click={() => clickMinusMaxTimePeriodForCashDesk(index)}>-</button>
					<button on:click={() => clickPlusMaxTimePeriodForCashDesk(index)}>+</button>
				</div>
			{/each}
		</div>
	</div>
</main>

<style>
	input {
		display: block;
	}

	.cashDesk input {
		width: 80%;
		margin: 0 auto;
	}

	.cashDesk {
		display: inline-block;
		width: 100px;
		height: 300px;
		background: black;
		margin-top: 50px;
		margin-left: 10px;
		color: white;
		text-align: center;
	}
</style>
