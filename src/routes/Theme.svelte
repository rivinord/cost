<script lang="ts">

    // add to tailwind config when needed to add new
    const themes = [
        { name: 'auto', label: 'Auto' },
        { name: 'light', label: 'Light' },
        { name: 'dark', label: 'Dark' },
        { name: 'retro', label: 'Retro' },
    ]

    let theme = $state('auto')

$effect(() => {
    document.documentElement.setAttribute('data-theme', theme)
    })

</script>

<form class="flex gap-5">
    {#each themes as { name, label } }
        <div class="inline-flex items-center gap-2">
            <input bind:group={theme} id={name} {name} value={name} type="radio" />
            <label for={name}>{label}</label>

        </div>
    {/each}

</form>





<div class="max-w-lg 3xl:max-w-xl">
    <h1 class="font-display text-4xl">
        <span class="text-neon-pink">Dark mode</span> works!!1
    </h1>
  </div>