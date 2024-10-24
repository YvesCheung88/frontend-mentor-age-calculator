<script lang="ts">
	const inputs: Record<string, string> = {
		day: '',
		month: '',
		year: ''
	};

	const errors: Record<string, string> = {
		day: '',
		month: '',
		year: ''
	};
	const outputLabels: Record<string, string> = {
		years: '--',
		months: '--',
		days: '--'
	};
	$: error = Object.values(errors).join('') !== '';

	function calculate() {
		Object.entries(inputs)
			.reverse()
			.forEach(([fieldName, value]) => {
				const parsedValue = parseInt(value);
				if (isNaN(parsedValue)) errors[fieldName] = `Must be a valid ${fieldName}`;
			});
	}
</script>

<form class="calculator" on:submit|preventDefault={calculate}>
	<section class="inputs" class:error>
		{#each Object.keys(inputs) as fieldName}
			<label for={fieldName}>{fieldName}</label>
			<input type="text" id={fieldName} bind:value={inputs[fieldName]} />
			<span>{errors[fieldName]}</span>
		{/each}
	</section>
	<section class="control">
		<button type="submit">
			<img src="/icon-arrow.svg" alt="icon-arrow" />
		</button>
	</section>
	<section class="outputs">
		{#each Object.entries(outputLabels) as [fieldName, value]}
			<p><span>{value}</span>{fieldName}</p>
		{/each}
	</section>
</form>

<style lang="postcss">
	.calculator {
		@apply border p-14 rounded-3xl;
		width: 840px;
		border-bottom-right-radius: 200px;
		border-color: var(--light-grey);
		background-color: var(--white);
		font-family: 'Poppins';

		& .inputs {
			@apply grid grid-cols-3 gap-x-8 gap-y-2;
			& label {
				@apply row-start-1 uppercase font-bold;
				letter-spacing: 3.5px;
			}
			& input {
				@apply row-start-2 rounded-lg px-6 py-3 border outline-none cursor-pointer;
				border-color: var(--light-grey);
				&:focus {
					border-color: var(--purple);
				}
			}
			& span {
				@apply row-start-3 italic font-normal;
			}
			&.error {
				color: var(--light-red);
			}
		}
		& .control {
			@apply grid items-center;
			grid-template-columns: auto 96px;
			&&::before {
				@apply w-full border;
				content: '';
			}
			& button {
				@apply w-24 h-24 rounded-full grid place-items-center hover:bg-black;
				background-color: var(--purple);
			}
		}
		& .outputs {
			@apply italic font-extrabold;
			font-size: 104px;
			line-height: 110%;
			& span {
				color: var(--purple);
			}
		}
	}
</style>
