<script lang="ts">
    import { Sun, Moon } from 'lucide-svelte';


    // Инициализация темы, 'dark' по умолчанию
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
        console.log('localstorageSetTo', {theme});
    });
</script>

<label class="swap swap-rotate">
    <!-- this hidden checkbox controls the state -->
    <input type="checkbox"  />
  
    <!-- sun icon -->
    <Sun class="swap-on" />
  
    <!-- moon icon -->
    <Moon class="swap-off" />
  </label>
