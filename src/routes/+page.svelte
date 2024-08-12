<script lang="ts">
	import { error } from '@sveltejs/kit';

	let day = '';
	let month = '';
	let year = '';

	let dayLabel = '--';
	let monthLabel = '--';
	let yearLabel = '--';
	let errorYear: string | undefined;
	let errorMonth: string | undefined;
	let errorDay: string | undefined;

	function buttonOnClick() {
		if (isValidYear(year) && isValidMonth(month) && isValidDay(day)) {
			yearLabel = `${calculateYear(year)}`;
			dayLabel = `${calculateDay(day)}`;
			monthLabel = `${calculateMonth(month)}`;
		} else {
			yearLabel = '--';
			dayLabel = '--';
			monthLabel = '--';
		}
	}
	function isValidYear(year: number): boolean {
		errorYear = undefined;
		const minYear = 1900;
		const maxYear = new Date().getFullYear();
		const isValid = year >= minYear && year <= maxYear;
		if (!isValid) {
			errorYear = 'Invalid yeahhhh';
		}

		return isValid;
	}
	function isValidMonth(month: number): boolean {
		errorMonth = undefined;
		const minMonth = 1;
		const maxMonth = 12;
		const isValid = month >= minMonth && month <= maxMonth;
		if (!isValid) {
			errorMonth = 'Invalid month';
		}
		return isValid;
	}
	function isValidDay(day: number): boolean {
		errorDay = undefined;
		const minDay = 1;
		const maxDay = 31;
		const isValid = day >= minDay && day <= maxDay;
		if (!isValid) {
			errorDay = 'Invalid day';
		}
		return isValid;
	}
	function calculateYear(year: number) {
		const currentDate = new Date();
		const currentYear = currentDate.getFullYear();
		const calculateYear = currentYear - year;
		return calculateYear;
	}
	function calculateMonth(month: number) {
		const currentDate = new Date();
		const currentMonth = currentDate.getMonth();
		const calculateMonth = currentMonth - month + 1;
		return calculateMonth;
	}

	function calculateDay(day: number) {
		const currentDate = new Date();
		const currentDay = currentDate.getDate();
		const calculateDay = currentDay - day;
		return calculateDay;
	}
</script>

<div class="calculator-container">
	<div class="inputs error">
		<label>
			<span>day</span>
			<input type="text" placeholder="DD" bind:value={day} />
			<span>This field is required</span>
		</label>
		<label>
			<span>month</span>
			<input type="text" placeholder="MM" bind:value={month} />
			<span>This field is required</span>
		</label>
		<label>
			<span>year</span>
			<input type="text" placeholder="YYYY" bind:value={year} />
			<span>This field is required</span>
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
	}
</style>
