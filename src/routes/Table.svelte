<script lang="ts">

import { X, Plus }  from 'lucide-svelte';


let examples = [
	{ name: 'Молоко', label: '🥛 Молоко' },
	{ name: 'Хлеб', label: '🍞 Хлеб' },
	{ name: 'Масло', label: '🧈 Масло' },
	{ name: 'Яйцо', label: '🥚 Яица' },
	{ name: 'Сыр', label: '🧀 Сыр' },
	{ name: 'Мясо', label: '🥩 Мясо' },
	{ name: 'Рыба', label: '🐟 Рыба' },
	{ name: 'Картофель', label: '🥔 Картофель' },
	{ name: 'Помидор', label: '🍅 Помидор' },
	{ name: 'Лук', label: '🥒 Лук' },
	{ name: 'Чеснок', label: '💡 Чеснок' },
	{ name: 'Соль', label: '💸 Соль' },
	{ name: 'Сахар', label: '🍮 Сахар' },
	{ name: 'Мед', label: '🍯 Мед' },
	{ name: 'Оливковое масло', label: '💧 Оливковое масло' },
]

let ingredients = $state([
		{ id: 1, name: 'Помидор', packageCost: 2, packageVolume: 3.5, recipeVolume: 4 },
		{ id: 2, name: 'Лук', packageCost: 1.2, packageVolume: 1.5, recipeVolume: 2 },
		{ id: 3, name: 'Картошка', packageCost: 0.5, packageVolume: 0.5, recipeVolume: 0.5 },
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


<div class=" grid grid-cols-1 gap-2 p-6">
    <p>Добавьте ингредиенты в таблицу, например:</p>
    <!-- render examples -->

    <div class="flex flex-wrap  gap-2 justify-stretch">
        {#each examples as { name, label }}
        <button class="btn btn-sm" onclick={() => (ingredients = [...ingredients, { id: ingredients.length + 1, name, packageCost: 0, packageVolume: 0, recipeVolume: 0 }])}>{label}</button>
        {/each}
    </div>

    <p>и введите стоимость и объем каждого ингредиента в упаковке и рецепте</p>
</div>

<div class="grid grid-cols-1 gap-7">
    
    <h1 class="text-3xl font-bold px-6 ">Ingredients table</h1>

    <!-- table div starts here -->
    <div class="border rounded-2xl overflow-hidden mx-2">
        <table class="table">
            <thead class="bg-base-200">
                <tr>
                    <th scope="col"></th>
                    <th scope="col">🍱 Ингредиент</th>
                    <th scope="col">Стоимость упаковки</th>
                    <th scope="col">Объем в&nbsp;упаковке</th>
                    <th scope="col">Объем в&nbsp;рецепте</th>
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
                        <!-- кнопка удалить ингредиент -->
                        <td class="bg-base-200"><button  onclick={() => deleteIngredient(ingredient.id)}><X class="text-red-500" strokeWidth={3} size={20} /></button></td>
                    </tr>
                {/each}
            </tbody>
            <tfoot>
                <tr class="bg-base-200">
                    <td colspan="2"></td>
                    <!-- кнопка добавить новый ингредиент -->
                    <!-- <td colspan="1"><button class="btn btn-outline w-full border-dashed border-base-content/30" onclick={() => (ingredients = [...ingredients, { id: ingredients.length + 1, name: '', packageCost: 0, packageVolume: 0, recipeVolume: 0 }])}> 
                        <Plus /> 
                    </button></td> -->
                    <!-- выводит сумму всех ингредиентов в рецепте -->
                    <td class="text-right text-red-500 text-lg" colspan="3">
                        Итого:
                        {tableSum()}&#8381;
                    </td>
                    <td colspan="1"></td>
                </tr>
            </tfoot>
        </table>
</div>



<!-- кнопка добавить новый ингредиент -->
<button class="btn" onclick={() => (ingredients = [...ingredients, { id: ingredients.length + 1, name: '', packageCost: 0, packageVolume: 0, recipeVolume: 0 }])}>
    Add new 
</button>

<button class="btn btn-primary" onclick={() => {
    ingredients = [{ id: 1, name: '', packageCost: 0, packageVolume: 0, recipeVolume: 0 }];
}}>
    Clear and Add New
</button>


</div>


<style>

    input {
    @apply input-bordered border-dashed text-sm;
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