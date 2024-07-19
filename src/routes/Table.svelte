<script lang="ts">

import { X }  from 'lucide-svelte';

let ingredients = $state([
		{ id: 1, name: 'Tomato', packageCost: 2, packageVolume: 3.5, recipeVolume: 4 },
		{ id: 2, name: 'Onion', packageCost: 1.2, packageVolume: 1.5, recipeVolume: 2 },
		{ id: 3, name: 'Potato', packageCost: 0.5, packageVolume: 0.5, recipeVolume: 0.5 },
	]);


    const deleteIngredient = (id: number) => {
		ingredients = ingredients.filter(ingredient => ingredient.id !== id);
		ingredients = ingredients.map((ingredient, index) => ({
			...ingredient,
			id: index + 1
		}));
	};

    const tableSum = $derived(() => {
		return ingredients
			.reduce((total, ingredient) => {
				if (ingredient.packageCost > 0 && ingredient.packageVolume > 0 && ingredient.recipeVolume > 0) {
					return total + ingredient.packageCost * (ingredient.recipeVolume / ingredient.packageVolume);
				}
				return total;
			}, 0)
			.toFixed(2);
	});

</script>




<h1 class="text-3xl font-bold ml-4">Ingredients table</h1>

<div class="border rounded-lg overflow-hidden">
<table class="table">
	<thead>
		<tr>
			<th scope="col"></th>
			<th scope="col">Ingredient</th>
			<th scope="col">Package Cost</th>
			<th scope="col">Package Volume</th>
			<th scope="col">Recipe Volume</th>
			<th scope="col"></th>
		</tr>
	</thead>
	<tbody>
		{#each ingredients as ingredient (ingredient.id)}
			<tr>
                <th class="text-xs bg-base-200 text-neutral w-fit">{ingredient.id}</th>
				<th scope="row"><input type="text" bind:value={ingredient.name} class="input input-bordered w-full max-w-xs" /></th>
				<td><input type="number" bind:value={ingredient.packageCost} class="input input-bordered w-full max-w-xs" /></td>
				<td><input type="number" bind:value={ingredient.packageVolume} class="input input-bordered w-full max-w-xs" /></td>
				<td><input type="number" bind:value={ingredient.recipeVolume} class="input input-bordered w-full max-w-xs" /></td>
				<td><button onclick={() => deleteIngredient(ingredient.id)}><X class="text-red-500" strokeWidth={3} size={20} /></button></td>
			</tr>
		{/each}
	</tbody>
    <tfoot>
        <tr class="bg-base-200">
            <td class="text-right text-red-500  text-xl" colspan="6">Общая себестоимость: {tableSum()} ₽</td>
        </tr>
    </tfoot>
</table>
</div>

<p>rrr</p>

<button class="btn" onclick={() => (ingredients = [...ingredients, { id: ingredients.length + 1, name: '', packageCost: 0, packageVolume: 0, recipeVolume: 0 }])}> Add new </button>


<style>




  table {
    margin: 0rem; /* Переопределить margin для table на 0 */
  }

    input {
    @apply input-bordered border-dashed;
    }

    .table td, .table th {
      padding: 0.25rem;
      white-space: normal; /* Позволяет тексту переноситься на новые строки */
      word-wrap: break-word; /* Переносит слова при необходимости */
    }
  
    .table input {
      width: 100%;
      box-sizing: border-box;
      padding: 0.5rem;
    }

  </style>