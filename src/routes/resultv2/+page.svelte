<script>
	import { Button } from 'flowbite-svelte';
	import { goto } from '$app/navigation';
	import UserInfos from '../../Store.js';
	let infos = {}

	UserInfos.subscribe((data) => {
		infos = (data);
	})

	const getFullName = () => {
		return `${infos?.firstName} ${infos?.lastName}`
	}
	const getCarFullInfo = () => {
		return `${infos.carModel} ${infos.carBrand}`
	}

</script>
<div class='main'>
	<div>
		<h1 class='font-bold text-3xl'>Result V2</h1>
		<div class='mt-5 w-50'>
			<h4><strong>Full name:</strong> <span> { getFullName() } </span></h4>
			<h4><strong>Car:</strong> <span> { getCarFullInfo() } </span></h4>
			<h4><strong>Zipcode:</strong> <span>{infos.zipCode}</span></h4>
			<h4><strong>First registration:</strong> <span>{infos.firstRegistration}</span></h4>
			<Button class="dark:text-white w-full mt-10" on:click={() => goto('/resultv1')}>Back to ResultV1</Button>
		</div>
	</div>
</div>

<style lang='scss'>
  .main {
    display: flex;
    justify-content: center;
    padding: 100px 0;
  }
</style>
