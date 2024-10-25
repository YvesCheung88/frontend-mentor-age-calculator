<script lang="ts">
	let day = '';
	let month = '';
	let year = '';

	let dayLabel = '--';
	let monthLabel = '--';
	let yearLabel = '--';

	let errorYear = '';
	let errorMonth = '';
	let errorDay = '';

	$: hasError = errorYear !== '' || errorMonth !== '' || errorDay !== '';
	function buttonOnClick() {
		let _isValidYear = isValidYear(year);
		let _isValidMonth = isValidMonth(month);
		let _isValidDay = isValidDay(day);
		if (_isValidYear && _isValidMonth && _isValidDay) {
			let calculatedYear = calculateYear(year);
			let calculatedMonth = calculateMonth(month);
			let calculatedDay = calculateDay(day);

			if (calculatedDay < 0) {
				calculatedMonth = calculatedMonth - 1;
				const currentDate = new Date();
				const lastMonth = new Date(currentDate.getFullYear(), currentDate.getMonth(), 0);
				calculatedDay += lastMonth.getDate();
			}
			if (calculatedMonth < 0) {
				calculatedYear = calculatedYear - 1;
				calculatedMonth = calculatedMonth + 12;
			}

			yearLabel = `${calculatedYear}`;
			dayLabel = `${calculatedDay}`;
			monthLabel = `${calculatedMonth}`;
		} else {
			yearLabel = '--';
			dayLabel = '--';
			monthLabel = '--';
		}
	}
	function isValidYear(year: string): boolean {
		errorYear = '';
		let parsedValue = parseInt(year);
		if (isNaN(parsedValue)) {
			errorYear = 'Must be a valid number';
			return false;
		}
		const minYear = 1900;
		const maxYear = new Date().getFullYear();
		const isValid = parsedValue >= minYear && parsedValue <= maxYear;
		if (!isValid) {
			errorYear = 'Must be in the past ';
		}
		return isValid;
	}
	function isValidMonth(month: string): boolean {
		errorMonth = '';
		let parsedValue = parseInt(month);
		if (isNaN(parsedValue)) {
			errorMonth = 'Must be a valid number';
			return false;
		}
		const minMonth = 1;
		const maxMonth = 12;
		const isValid = parsedValue >= minMonth && parsedValue <= maxMonth;
		if (!isValid) {
			errorMonth = 'Must be a valid month';
		}
		return isValid;
	}
	function isValidDay(day: string): boolean {
		errorDay = '';
		let parsedValue = parseInt(day);
		if (isNaN(parsedValue)) {
			errorDay = 'Must be a valid number';
			return false;
		}
		const minDay = 1;
		const maxDay = 31;
		const isValid = parsedValue >= minDay && parsedValue <= maxDay;
		if (!isValid) {
			errorDay = 'Must be a valid day';
		}
		return isValid;
	}
	function calculateYear(year: string) {
		const currentDate = new Date();
		const currentYear = currentDate.getFullYear();
		return currentYear - parseInt(year);
	}
	function calculateMonth(month: string) {
		const currentDate = new Date();
		const currentMonth = currentDate.getMonth();
		return currentMonth - parseInt(month) + 1;
	}

	function calculateDay(day: string) {
		const currentDate = new Date();
		const currentDay = currentDate.getDate();
		return currentDay - parseInt(day);
	}
</script>

<main>
	<div class="calculator-container">
		<div class="inputs" class:error={hasError}>
			<label>
				<span>day</span>
				<input type="text" placeholder="DD" bind:value={day} />
				<span>{errorDay}</span>
			</label>
			<label>
				<span>month</span>
				<input type="text" placeholder="MM" bind:value={month} />
				<span>{errorMonth}</span>
			</label>
			<label>
				<span>year</span>
				<input type="text" placeholder="YYYY" bind:value={year} />
				<span>{errorYear}</span>
			</label>
		</div>
		<div class="relative flex justify-center md:justify-end">
			<hr class="absolute top-1/2 w-full" />
			<button on:click={buttonOnClick}>
				<img src="/icon-arrow.svg" alt="button" />
			</button>
		</div>
		<div class="output">
			<span><span>{yearLabel}</span> years</span>
			<span><span>{monthLabel}</span> months</span>
			<span><span>{dayLabel}</span> days</span>
		</div>
	</div>
</main>

<style lang="postcss">
	main {
		@apply h-screen grid place-content-center bg-gray-100;
		& .calculator-container {
			@apply bg-white rounded-[24px_24px_200px_24px] m-3 p-6;
			& .inputs {
				@apply grid grid-cols-3 gap-4 mb-8;
				& label {
					@apply flex flex-col  justify-between text-[var(--smokey-grey)] uppercase font-bold;
					& input {
						@apply p-3 rounded-xl border border-[var(--smokey-grey)] text-3xl cursor-pointer outline-[var(--purple)];
					}
					& span:first-child {
						@apply tracking-[3.5px] mb-1;
					}
				}
				&&.error {
					& label {
						@apply text-[var(--light-red)];
						& input {
							@apply border border-[var(--light-red)];
						}
					}
				}
			}
			& button {
				@apply grid place-items-center bg-[var(--purple)] rounded-full h-14 w-14 z-10;
				& img {
					@apply w-6 h-6;
				}
				&:hover {
					@apply bg-[var(--off-black)];
				}
			}
			& .output {
				@apply grid leading-[110%] text-6xl italic font-extrabold tracking-[-2.08px] my-6;
				& > span > span {
					@apply text-[var(--purple)] text-6xl;
				}
			}
		}
	}
</style>
