<script lang="ts">

import { X, Plus }  from 'lucide-svelte';


let examples = [
	{ name: 'Milk', label: '🥛 Milk' },
	{ name: 'Bread', label: '🍞 Bread' },
	{ name: 'Butter', label: '🥖 Butter' },
	{ name: 'Egg', label: '🥚 Egg' },
	{ name: 'Cheese', label: '🧀 Cheese' },
	{ name: 'Meat', label: '🥩 Meat' },
	{ name: 'Fish', label: '🐟 Fish' },
	{ name: 'Fruits', label: '🍉 Fruits' },
	{ name: 'Vegetables', label: '🥕 Vegetables' },
	{ name: 'Pasta', label: '🍝 Pasta' },
	{ name: 'Rice', label: '🍚 Rice' },
	{ name: 'Potato', label: '🥔 Potato' },
	{ name: 'Tomato', label: '🍅 Tomato' },
	{ name: 'Onion', label: '🥒 Onion' },
	{ name: 'Garlic', label: '💡 Garlic' },
	{ name: 'Herbs', label: '🌿 Herbs' },
	{ name: 'Spices', label: '❗️ Spices' },
	{ name: 'Salt', label: '💸 Salt' },
	{ name: 'Sugar', label: '🍮 Sugar' },
	{ name: 'Honey', label: '🍯 Honey' },
	{ name: 'Olive oil', label: '💧 Olive oil' },
	{ name: 'Other', label: '🤔 Other' },
]

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


<div class="flex flex-wrap gap-2">
    {#each examples as { name, label }}
    <button class="btn">{label}</button>
    {/each}
</div>


<h1 class="text-3xl font-bold ml-4 ">Ingredients table</h1>

<div class="border rounded-2xl overflow-hidden">
<table class="table">
	<thead class="bg-base-200">
		<tr>
			<th scope="col"></th>
			<th scope="col">Ингредиент</th>
			<th scope="col">Стоимость упаковки</th>
			<th scope="col">Объем в упаковке</th>
			<th scope="col">Объем в рецепте</th>
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
				<td class="bg-base-200"><button  onclick={() => deleteIngredient(ingredient.id)}><X class="text-red-500" strokeWidth={3} size={20} /></button></td>
			</tr>
		{/each}
	</tbody>
    <tfoot>
        <tr class="bg-base-200">
            <td colspan="1"></td>
            <td colspan="1"><button class="btn btn-outline w-full border border-dashed border-neutral" onclick={() => (ingredients = [...ingredients, { id: ingredients.length + 1, name: '', packageCost: 0, packageVolume: 0, recipeVolume: 0 }])}> 
                <Plus /> 
            </button></td>
            <td class="text-right text-red-500 text-lg" colspan="3">
                Общая себестоимость:
                {tableSum()}&#8381;
            </td>
            <td colspan="1"></td>
        </tr>
    </tfoot>
</table>
</div>

<p>rrr 
</p>

<button class="btn" onclick={() => (ingredients = [...ingredients, { id: ingredients.length + 1, name: '', packageCost: 0, packageVolume: 0, recipeVolume: 0 }])}>
    Add new 
</button>


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

    div {
      border-color: transparent;
    }
    

  </style>