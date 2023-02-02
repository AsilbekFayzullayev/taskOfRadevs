<script>
	import { Input, Button, Label, Badge, Checkbox } from 'flowbite-svelte';
	import { createEventDispatcher } from 'svelte';
	import * as yup from 'yup'
	import UserInfos from '../Store.js';
	import { goto } from '$app/navigation';
	const schema = yup.object().shape({
		firstName: yup.string().required('Required'),
		lastName: yup.string().required('Required'),
		carModel: yup.string().required('Required')
	});
	let values = {
		firstRegistration: false
	};
	let errors = {};
	const dispatch = createEventDispatcher();
	function changeStep() {
		dispatch('message', {
			value: false
		});
	}
	function addDataToStore() {
		UserInfos.update((currentData) => {
			return { ...values, ...currentData }
		})
	}
	async function submitHandler() {
		try {
			// `abortEarly: false` to get all the errors
			await schema.validate(values, { abortEarly: false });
			addDataToStore();
			await goto('/resultv1');
			errors = {};
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
<form class='w-full' on:submit|preventDefault={submitHandler}>
	<div class="mb-6">
		<Label for="car_brand" class="mb-2">First name</Label>
		<Input type="text" id='car_brand' placeholder="Type..." bind:value={values.firstName}/>
		{#if errors.firstName}
			<Badge color="red" class='w-full text-left'>{errors.firstName}</Badge>
		{/if}
	</div>
	<div class="mb-6">
		<Label for="car_brand" class="mb-2">Last name</Label>
		<Input type="text" id='car_brand' placeholder="Type..." bind:value={values.lastName}/>
		{#if errors.lastName}
			<Badge color="red" class='w-full text-left'>{errors.lastName}</Badge>
		{/if}
	</div>
	<div class="mb-6">
		<Label for="car_brand" class="mb-2">Car model</Label>
		<Input type="text" id='car_brand' placeholder="Type..." bind:value={values.carModel}/>
		{#if errors.carModel}
			<Badge color="red" class='w-full text-left'>{errors.carModel}</Badge>
		{/if}
	</div>
	<div class="mb-6">
		<Checkbox on:change={(e) => values.firstRegistration = (e.target.checked)} bind:value={values.firstRegistration}>First registration ?({values.firstRegistration ? 'Yes' : 'No'})</Checkbox>
	</div>
	<div class='flex'>
		<Button class="dark:text-white w-1/2" on:click={changeStep}>
			<span>Back</span>
		</Button>
		<Button class="dark:text-white w-1/2 ml-1" type="submit">
			<span>Next</span>
		</Button>
	</div>
</form>

