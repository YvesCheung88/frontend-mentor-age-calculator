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

	$: hasError = errorYear !== '' && errorMonth !== '' && errorDay !== '';
	function buttonOnClick() {
		let _isValidYear = isValidYear(year);
		let _isValidMonth = isValidMonth(month);
		let _isValidDay = isValidDay(day);
		if (_isValidYear && _isValidMonth && _isValidDay) {
			yearLabel = `${calculateYear(year)}`;
			dayLabel = `${calculateDay(day)}`;
			monthLabel = `${calculateMonth(month)}`;
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
	<button on:click={buttonOnClick}>
		<img src="/icon-arrow.svg" alt="button" />
	</button>
	<div class="flex flex-col">
		<span class="output year"> {yearLabel} years</span>
		<span class="output month">{monthLabel} months</span>
		<span class="output day">{dayLabel} days</span>
	</div>
</div>

<style lang="postcss">
	.calculator-container {
		width: 840px;
		height: 651px;
		background-color: white;
		border-radius: 24px 24px 200px 24px;
		padding: 56px;
		& .inputs {
			display: flex;
			gap: 32px;
			& label {
				@apply text-gray-500 uppercase font-bold;
				display: flex;
				flex-direction: column;
				width: 160px;
				justify-content: space-between;
				& input {
					padding: 12px 24px;
					border-radius: 8px;
					border: 1px solid gray;
				}
				& span:last-child {
					font-style: italic;
					font-size: 14px;
					font-weight: normal;
					text-transform: none;
					height: 21px;
				}
				& span:first-child {
					letter-spacing: 3.5px;
				}
			}
			&&.error {
				& label {
					color: red;
					& input {
						border-color: red;
					}
				}
			}
		}
		& button {
			background-color: hsl(259, 100%, 65%);
			border-radius: 100%;
			height: 96px;
			width: 96px;
			display: grid;
			place-items: center;
		}
	}
</style>
