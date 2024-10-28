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
				calculatedMonth -= 1;
				const currentDate = new Date();
				const lastMonth = new Date(currentDate.getFullYear(), currentDate.getMonth(), 0);
				calculatedDay += lastMonth.getDate();
			}
			if (calculatedMonth < 0) {
				calculatedYear -= 1;
				calculatedMonth += 12;
			}
			yearLabel = `${calculatedYear}`;
			dayLabel = `${calculatedDay}`;
			monthLabel = `${calculatedMonth}`;
		} else {
			resetLabels();
		}
	}
	function isValidYear(year: string): boolean {
		errorYear = '';
		let parsedValue = parseInt(year);
		if (year.trim() === '') {
			errorYear = 'The field is required';
			return false;
		}
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
		const currentYear = new Date().getFullYear();
		return currentYear - parseInt(year);
	}
	function calculateMonth(month: string) {
		const currentMonth = new Date().getMonth();
		return currentMonth - parseInt(month) + 1;
	}

	function calculateDay(day: string): number {
		return new Date().getDate() - parseInt(day);
	}
	function resetLabels() {
		yearLabel = '--';
		dayLabel = '--';
		monthLabel = '--';
	}
</script>

<main>
	<div class="calculator-container">
		<div class="inputs" class:error={hasError}>
			<label>
				<span>DAY</span>
				<input type="text" placeholder="DD" bind:value={day} />
				<span>{errorDay}</span>
			</label>
			<label>
				<span>MONTH</span>
				<input type="text" placeholder="MM" bind:value={month} />
				<span>{errorMonth}</span>
			</label>
			<label>
				<span>YEAR</span>
				<input type="text" placeholder="YYYY" bind:value={year} />
				<span>{errorYear}</span>
			</label>
		</div>
		<div class="button-wrapper">
			<hr class="custom-hr" />
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
				@apply grid grid-cols-3 gap-4 my-6 md:grid-cols-4 md:my-4;
				& label {
					@apply flex flex-col  justify-between text-[var(--smokey-grey)]  font-bold;

					& input {
						@apply p-3 mt-1 rounded-xl border border-[var(--smokey-grey)] text-2xl cursor-pointer outline-[var(--purple)] md:w-32 text-black;
					}
					& span:first-child {
						@apply tracking-[3.5px];
					}
					& span:last-child {
						@apply text-sm font-normal mt-1 h-4;
					}
				}
				&.error label {
					@apply text-[var(--light-red)];
					& input {
						@apply border border-[var(--light-red)];
					}
				}
			}
			& .button-wrapper {
				@apply relative flex justify-center md:justify-end;
				& .custom-hr {
					@apply absolute top-1/2 w-full;
				}
				& button {
					@apply grid place-items-center bg-[var(--purple)] rounded-full p-5 z-10 md:p-6;
					& img {
						@apply size-6 md:size-7;
					}
					&:hover {
						@apply bg-[var(--off-black)];
					}
				}
			}
			& .output {
				@apply grid leading-[110%] text-6xl italic font-extrabold tracking-[-2.08px] my-7 md:my-4;
				& > span > span {
					@apply text-[var(--purple)] text-6xl;
				}
			}
		}
	}
</style>
