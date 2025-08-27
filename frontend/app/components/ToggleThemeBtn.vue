<template>
    <v-btn
        :icon="isDark ? 'mdi-weather-night' : 'mdi-weather-sunny'"
        @click="handleToggleTheme"
    />
</template>

<script setup lang="ts">
import { useTheme } from 'vuetify/framework';

const theme = useTheme();
const isDark = computed(() => theme.global.name.value === 'dark');

function handleToggleTheme(event: MouseEvent) {
    let x, y: number;
    if (import.meta.dev) {
        x = event.clientX;
        y = event.clientY;
    } else {
        const button = event.currentTarget as HTMLElement;
        const rect = button.getBoundingClientRect();
        x = rect.left + rect.width / 2;
        y = rect.top + rect.height / 2;
    }

    const endRadius = Math.hypot(
        Math.max(x, innerWidth - x),
        Math.max(y, innerHeight - y),
    );

    if (!document.startViewTransition) {
        toggleTheme();
        return;
    }

    const transition = document.startViewTransition(() => {
        toggleTheme();
    });

    transition.ready.then(() => {
        const clipPath = [
            `circle(0px at ${x}px ${y}px)`,
            `circle(${endRadius}px at ${x}px ${y}px)`,
        ];
        document.documentElement.animate(
            { clipPath: isDark.value ? [...clipPath].reverse() : clipPath },
            {
                duration: 400,
                easing: 'ease-in',
                pseudoElement: isDark.value
                    ? '::view-transition-old(root)'
                    : '::view-transition-new(root)',
            },
        );
    });
}

function toggleTheme() {
    theme.toggle();
    if (isDark.value) {
        document.documentElement.classList.add('dark');
    } else {
        document.documentElement.classList.remove('dark');
    }
}

onMounted(() => {
    if (isDark.value) {
        document.documentElement.classList.add('dark');
    } else {
        document.documentElement.classList.remove('dark');
    }
});
</script>

<style scoped>

</style>
