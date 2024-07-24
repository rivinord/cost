<script lang="ts">
    // Добавьте список доступных тем
    const themes = [
        // { name: 'auto', label: 'Auto' },
        { name: 'light', label: 'Светлая' },
        { name: 'dark', label: 'Тёмная' },
        { name: 'retro', label: 'Кекс' },
    ];

    // Инициализация темы, 'auto' по умолчанию
    let theme = $state('dark');

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




  <select class="select select-bordered w-full max-w-52" bind:value={theme}>
    {#each themes as theme}
      <option value={theme.name}>
        {theme.label}
      </option>
    {/each}
  </select>

  