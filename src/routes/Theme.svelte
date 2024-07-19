<script lang="ts">

    // add to tailwind config when needed to add new
    const themes = [
        { name: 'auto', label: 'Auto' },
        { name: 'light', label: 'Light' },
        { name: 'dark', label: 'Dark' },
        { name: 'retro', label: 'Retro' },
    ]

    //should be auto, light just when developing
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





