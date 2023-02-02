<script>
	import { Card, Input, Button, Label, Badge } from 'flowbite-svelte';
	import * as yup from 'yup'
	import SecondStepInSignUp from '../components/SecondStepInSignUp.svelte';
	import UserInfos from '../Store.js';
	import { goto } from '$app/navigation';

	const schema = yup.object().shape({
		carBrand: yup.string().test("isHaveCar", 'Car brand should be one of these Cars - (BMW, Audi, Nissan)', (value) => (['BMW', 'Audi', 'Nissan'].includes(value))),
		zipCode: yup.string().test("isHaveCode", "Zip code should be one of these codes -  (65000, 66000, 67000, 68000)", (value) => ([65000, 66000, 67000, 68000].includes(parseInt(value)))),
	});
	let values = {};
	let errors = {};
	let isValidOneStep = false
	function addDataToStore() {
		UserInfos.update((currentData) => {
			return { ...values, ...currentData }
		})
	}
	async function submitHandler() {
		try {
			await schema.validate(values, { abortEarly: false });
			errors = {};
			addDataToStore()
			isValidOneStep = true
		} catch (err) {
			errors = extractErrors(err);
		}
	}
	function extractErrors(err) {
		return err.inner.reduce((acc, err) => {
			return { ...acc, [err.path]: err.message };
		}, {});
	}
</script>
<div class='main'>
	<Card class='w-1/2'>
		<div class="flex flex-col items-center pb-4">
			<h1 class='text-2xl font-bold'>Sign Up</h1>
			{#if (errors.carBrand || errors.zipCode)}
				<Badge color="red" class='w-full text-left'>The brand or postal code is unfortunately not serviced.</Badge>
			{/if}
			<span class='text-sm'>
				{#if isValidOneStep} <span>step 2</span> {:else} <span>step 1</span> {/if}
			</span>
			{#if isValidOneStep}
				<SecondStepInSignUp on:message={(e) => isValidOneStep = e.detail.value}/>
			{:else}
				<form class='w-full' on:submit|preventDefault={submitHandler}>
					<div class="mb-6">
						<Label for="car_brand" class="mb-2">Car brand</Label>
						<Input type="text" id='car_brand' placeholder="Type..." bind:value={values.carBrand}/>
						{#if errors.carBrand}
							<Badge color="red" class='w-full text-left'>{errors.carBrand}</Badge>
						{/if}
					</div>
					<div class="mb-6">
						<Label for="zip_code" class="mb-2">Zip code</Label>
						<Input type="number" id='zip_code' placeholder="Type..." bind:value={values.zipCode}/>
						{#if errors.zipCode}
							<Badge color="red" class='w-full text-left'>{errors.zipCode}</Badge>
						{/if}
					</div>
					<Button class="dark:text-white w-full" type="submit">
						<span>Next</span>
					</Button>
				</form>
			{/if}
		</div>
	</Card>
</div>


<style lang='scss'>
	.main {
		display: flex;
		justify-content: center;
		padding: 100px 0;
	}
</style>
