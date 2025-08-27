<template>
    <v-layout>
        <v-system-bar
            class="overflow-hidden px-0"
            style="--wails-draggable:drag"
        >
            <v-btn
                class="wails-no-drag"
                icon="mdi-menu"
                size="x-small"
                variant="text"
                @click="drawerRef.show = !drawerRef.show"
            />
            <span
                class="mr-auto ms-2"
            >
                Wuchieh
            </span>
            <v-spacer />

            <toggle-theme-btn
                class="wails-no-drag mr-1"
                rounded="0"
                size="x-small"
            />

            <v-btn
                class="rounded-0 wails-no-drag"
                icon="mdi-minus"
                size="x-small"
                variant="text"
                @click="handleMinimize"
            />

            <v-btn
                class="rounded-0 wails-no-drag ms-2"
                icon="mdi-checkbox-blank-outline"
                size="x-small"
                variant="text"
                @click="handleToggleFullscreen"
            />

            <v-btn
                class="rounded-0 wails-no-drag ms-2"
                icon="mdi-close"
                size="x-small"
                variant="text"
                @click="handleClose"
            />
        </v-system-bar>

        <v-navigation-drawer
            v-model="drawerRef.show"
            :expand-on-hover="drawerRef.pin"
            :permanent="drawerRef.pin"
            :rail="drawerRef.pin"
        >
            <v-list>
                <v-list-item
                    :prepend-icon="drawerRef.pin ? 'mdi-pin' : 'mdi-pin-off'"
                    :title="drawerRef.pin ? '解除固定' : '固定'"
                    @click="drawerRef.pin = !drawerRef.pin"
                />
                <v-divider />
                <v-list-item
                    prepend-icon="mdi-home"
                    title="首頁"
                />
            </v-list>
        </v-navigation-drawer>

        <v-main>
            <slot />
        </v-main>
    </v-layout>
</template>

<script setup lang="ts">
import {
    Quit,
    WindowIsMaximised,
    WindowMaximise,
    WindowMinimise,
    WindowUnmaximise,
} from '~~/wailsjs/runtime';

const drawerRef = ref({
    pin: false,
    show: false,
});

function handleClose() {
    Quit();
}

function handleMinimize() {
    WindowMinimise();
}

async function handleToggleFullscreen() {
    const result = await WindowIsMaximised();
    if (result) {
        WindowUnmaximise();
    } else {
        WindowMaximise();
    }
}
</script>
