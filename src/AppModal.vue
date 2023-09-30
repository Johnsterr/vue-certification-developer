<script setup>
import { XMarkIcon } from "@heroicons/vue/20/solid";
defineProps({
    title: { type: String, default: null },
});

defineEmits(["close"]);
</script>

<template>
    <div
        class="absolute inset-0 flex items-center justify-center z-20 bg-gray-800/40 backdrop-blur"
    >
        <div
            class="relative shrink-0 w-full max-w-2xl rounded-md flex flex-col shadow-2xl bg-white dark:bg-gray-800 dark:text-white"
        >
            <button
                class="absolute -right-12 -top-2 text-gray-300 p-2 hover:text-white"
                @click="$emit('close')"
            >
                <XMarkIcon class="w-8 h-8" />
            </button>
            <h3
                class="w-full bg-white dark:bg-gray-800 dark:text-white p-4 rounded-t-md text-2xl"
                v-if="title"
            >
                {{ title }}
            </h3>
            <div class="p-4 space-y-4">
                <slot />
            </div>
        </div>
    </div>
</template>
