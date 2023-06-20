<script>
	import { goto } from '$app/navigation';
	import Button from '@smui/button';
</script>

<h1>This is the home page</h1>
<Button variant="outlined" style="border-color: #2171cd; background-color: #00BFFF; color: #FFFFFF" class="nav-button" on:click={() => goto('/form')} >Get Started</Button>


