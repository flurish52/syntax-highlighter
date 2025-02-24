<template>
    <div class="flex justify-between items-center bg-gray-800 text-white h-12 min-w-96 w-full">


        <!-- Empty right hand -->
        <div class="text-sm font-medium px-3 flex ">
            >
        </div>
        <!-- Centered Title -->
        <div class="text-sm font-medium px-3 flex ">
            <p class="h-fit">{{ languageName }}</p>
        </div>

        <!-- Window Controls (Right Side) -->
        <div class="ml-auto flex px-3 justify-between">
            <!-- Minimize Button -->
            <button class="mx-2 p-1 hover:bg-gray-700 rounded" title="Minimize">
                <Minus size="16"/>
            </button>
            <!-- Maximize Button -->
            <button class="mx-2 p-1 hover:bg-gray-700 rounded" title="Maximize">
                <Square size="16"/>
            </button>
            <!-- Close Button -->
            <button class="mx-2 p-1 hover:bg-red-700 bg-red-600 rounded" title="Close">
                <X size="16"/>
            </button>
        </div>

    </div>
</template>
<script setup>
import {X, Square, Minus} from 'lucide-vue-next';


let props = defineProps({
    languageName: String,
})

</script>
