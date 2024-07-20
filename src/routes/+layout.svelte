
<script>
  import "tailwindcss/tailwind.css";

 import Header from "./Header.svelte";

	let { children } = $props()
</script>

<svelte:head>
	<title>Daisy</title>
</svelte:head>


 <Header />


  {@render children()}


<style></style>

