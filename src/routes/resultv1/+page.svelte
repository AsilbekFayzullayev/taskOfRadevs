<script>
	import { Button } from 'flowbite-svelte';
	import UserInfos from '../../Store.js'
	import { goto } from '$app/navigation';
	let infos = {}

	UserInfos.subscribe((data) => {
		infos = (data);
	})


</script>
<div class='main'>
	<div>
		<h1 class='font-bold text-3xl'>Result V1</h1>
		<div class='mt-5 w-50'>
			<h4><strong>First name:</strong> <span>{infos.firstName}</span></h4>
			<h4><strong>Last name:</strong> <span>{infos.lastName}</span></h4>
			<h4><strong>Car model:</strong> <span>{infos.carModel}</span></h4>
			<h4><strong>Car brand:</strong> <span>{infos.carBrand}</span></h4>
			<h4><strong>Zipcode:</strong> <span>{infos.zipCode}</span></h4>
			<h4><strong>First registration:</strong> <span>{infos.firstRegistration}</span></h4>
			<Button class="dark:text-white w-full mt-10" on:click={() => goto('/resultv2')}>Result V2</Button>
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
