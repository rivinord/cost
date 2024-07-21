<script lang="ts">
	import { X, Plus } from 'lucide-svelte';
	import { fade } from 'svelte/transition';

	let examples = [
		{ name: 'Молоко', label: '🥛 Молоко' },
		{ name: 'Хлеб', label: '🍞 Хлеб' },
		{ name: 'Масло', label: '🧈 Масло' },
		{ name: 'Яица', label: '🥚 Яица' },
		{ name: 'Соль', label: '🧂 Соль' },
		{ name: 'Сахар', label: '⚪️ Сахар' },
		{ name: 'Перец', label: '⚫️ Перец' },
		{ name: 'Мед', label: '🍯 Мед' },
		{ name: 'Масло', label: '💧 Масло' }
	];

	let ingredients = $state([{ name: '', packageCost: '', packageVolume: '', recipeVolume: '' }]);

	//get ingredients from localStorage on launch
	$effect(() => {
		const savedIngredients = localStorage.getItem('ingredients');
		savedIngredients && (ingredients = JSON.parse(savedIngredients));
	});

	//реактивное изменение ингредиентов
	$effect(() => {
		localStorage.setItem('ingredients', JSON.stringify(ingredients));
	});

	const deleteIngredient = (i: number) => {
		ingredients = ingredients.filter((ingredient, index) => index !== i);
	};


	// const deleteIngredient = (id:number) => {
  	// 	ingredients = ingredients.filter((ingredient) => ingredient.id !== id);
	// };


	const tableSum = $derived(() => {
		return ingredients
			.reduce((total, ingredient) => {
				//сложная штука для типов чтобы по дефолту была пустая строка '', а проверка для начала счета через >0
				const packageCost = typeof ingredient.packageCost === 'string' ? Number(ingredient.packageCost) : ingredient.packageCost;
				const packageVolume = typeof ingredient.packageVolume === 'string' ? Number(ingredient.packageVolume) : ingredient.packageVolume;
				const recipeVolume = typeof ingredient.recipeVolume === 'string' ? Number(ingredient.recipeVolume) : ingredient.recipeVolume;

				if (packageCost > 0 && packageVolume > 0 && recipeVolume > 0) {
					return total + packageCost * (recipeVolume / packageVolume);
				}
				return total;
			}, 0)
			.toFixed(2);
	});
</script>

<!-- render examples -->
<div class="flex flex-wrap gap-2 p-6">
	{#each examples as { name, label }}
		<button class="btn btn-sm" onclick={() => ingredients.push({ name, packageCost: '', packageVolume: '', recipeVolume: '' })}
			>{label}</button>
	{/each}
</div>

<div class="grid grid-cols-1 gap-5">
	<div class="flex justify-start items-center px-6">
		<h1 class="text-3xl font-bold">Ингредиенты</h1>
		<!-- <button
			class="btn btn-xs ml-4"
			onclick={() => {
				ingredients = [{ id: 1, name: '', packageCost: '', packageVolume: '', recipeVolume: '' }];
			}}>
			Очистить
		</button> -->
	</div>
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
				<!-- redner ingredients table -->
				{#each ingredients as ingredient (ingredient)}
					<tr transition:fade>
						<th class="text-xs bg-base-200 text-neutral w-fit">{ingredients.indexOf(ingredient) + 1}</th>
						<th scope="row"><input type="text" bind:value={ingredient.name} class="input input-bordered w-full max-w-xs" /></th>
						<td><input type="number" bind:value={ingredient.packageCost} class="input input-bordered w-full max-w-xs" /></td>
						<td><input type="number" bind:value={ingredient.packageVolume} class="input input-bordered w-full max-w-xs" /></td>
						<td><input type="number" bind:value={ingredient.recipeVolume} class="input input-bordered w-full max-w-xs" /></td>
						<!-- кнопка удалить ингредиент -->
						<td class="bg-base-200">
							<button onclick={() => deleteIngredient(ingredients.indexOf(ingredient))}><X class="text-red-500" strokeWidth={3} size={20} /> </button>
						</td>
					</tr>
				{/each}
			</tbody>
			<tfoot>
				<tr class="bg-base-200">
					<td colspan="2"></td>
					<!-- кнопка добавить новый ингредиент -->
					<!-- <td colspan="1"><button class="btn btn-outline w-full border-dashed border-base-content/30" onclick={() => (ingredients = [...ingredients, { id: ingredients.length + 1, name: '', packageCost: '', packageVolume: '', recipeVolume: '' }])}> 
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
	<div class="flex justify-center gap-2">
		<button class="btn" onclick={() => ingredients.push({ name: '', packageCost: '', packageVolume: '', recipeVolume: '' })}>
			Добавить ингредиент
		</button>
	</div>
</div>

<style>
	input {
		@apply input-bordered border-dashed text-sm;
	}

	.table td,
	.table th {
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
