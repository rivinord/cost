<script lang="ts">
    // Добавьте список доступных тем
    const themes = [
        { name: 'auto', label: 'Auto' },
        { name: 'light', label: 'Light' },
        { name: 'dark', label: 'Dark' },
        { name: 'retro', label: 'Retro' },
    ];

    // Инициализация темы, 'auto' по умолчанию
    let theme = $state('auto');

    // Обновляем тему при загрузке страницы, если она сохранена в localStorage
    if (typeof window !== 'undefined') {
        const savedTheme = localStorage.getItem('theme');
        if (savedTheme) {
            theme = savedTheme;
        }
    }

    // Реактивно обновляем атрибут data-theme и localStorage при изменении theme
    $effect(() => {
        document.documentElement.setAttribute('data-theme', theme);
        localStorage.setItem('theme', theme);
    });
</script>

<form class="flex gap-5">
    {#each themes as { name, label } }
        <div class="inline-flex items-center gap-2">
            <input bind:group={theme} id={name} {name} value={name} type="radio" />
            <label for={name}>{label}</label>
        </div>
    {/each}
</form>
