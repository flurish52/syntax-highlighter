<script setup>
import { ref } from 'vue';
import { X, Square, Minus } from 'lucide-vue-next';

const windowTitle = ref('');
</script>

<template>
    <div class="flex justify-between items-center bg-gray-800 text-white h-10 w-full">
        <!-- Empty right hand -->
        <div class="text-sm font-medium px-3 flex ">
            >
        </div>

        <!-- Centered Title -->
        <div class="text-sm font-medium px-3 text-center">
            <input
                type="text "
                v-model="windowTitle"
            class="bg-transparent h-10"
                placeholder="Window Title"
            >
        </div>

        <!-- Window Controls (Right Side) -->
        <div class="ml-auto flex px-3 justify-between">
            <!-- Minimize Button -->
            <button class="mx-2 p-1 hover:bg-gray-700 rounded" title="Minimize">
                <Minus size="16" />
            </button>
            <!-- Maximize Button -->
            <button class="mx-2 p-1 hover:bg-gray-700 rounded" title="Maximize">
                <Square size="16" />
            </button>
            <!-- Close Button -->
            <button class="mx-2 p-1 hover:bg-red-700 bg-red-600 rounded" title="Close">
                <X size="16" />
            </button>
        </div>

    </div>
</template>

<style>
/* Ensuring the title bar looks native */
</style>
