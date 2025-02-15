<template>
    <div class="relative py-4 p-4 bg-gray-900 h-screen">

        <div class="w-3xl md:flex md:w-screen mx-auto  rounded-lg shadow-lg justify-center items-start item-start">
            <!-- Input Area -->
            <textarea
                v-model="code"
                placeholder="Paste your code here..."
                class="w-1/3  h-screen p-3 text-white bg-gray-800 border border-gray-700 rounded-lg font-mono focus:outline-none"
            ></textarea>

            <!-- Code Preview with Highlighting -->
                <pre
                    ref="codeContainer"
                    class="rounded-lg w-full h-full ml-2 flex flex-col bg-gray-900">
                    <TitleBar
                     @languageSelected="handleLanguageSelection"
                    />
                    <code :class="[language, 'w-fit p-3 bg-gray-900']"
                      v-html="Prism.highlight(code, Prism.languages.css, codelang)"></code>
                </pre>

        </div>

        <div

            class="flex gap-4 fixed top-2 right-1/3 ">
            <!-- Reset Button -->
            <button
                class="px-6 py-2 rounded-lg bg-red-500 text-white font-semibold shadow-md hover:bg-red-600 transition"
                @click="resetScreen()">
                Reset screen
            </button>

            <!-- Save as Image Button -->
            <button
                class="px-6 py-2 rounded-lg bg-blue-600 text-white font-semibold shadow-md hover:bg-blue-700 transition"
                @click="saveAsImage()">
                Save as Image
            </button>
        </div>

    </div>
</template>

<script setup>
import TitleBar from "@/Components/TitleBar.vue";
import {ref, watch, onMounted} from 'vue';
import Prism from 'prismjs';
import html2canvas from "html2canvas";

const code = ref('');
let codeContainer = ref('');

let codelang = ref('')
let language = ref('language-'+codelang)
let valueSelected  = ref();


let prism = ref(Prism.languages.css)
const updateHighlight = () => {
    setTimeout(() => {
        Prism.highlightAll();
    }, 100);
};

const handleLanguageSelection = (selectedValue) => {
    language.value = 'language-'+selectedValue;
    codelang.value = selectedValue;
    valueSelected.value = selectedValue;
    console.log(selectedValue)

    prism.value = 'Prism.languages.'+codelang.value
    updateHighlight()
};
const saveAsImage = () => {
    if (code.value !== '' && valueSelected.value !== '' ) {
        html2canvas(codeContainer.value, {backgroundColor: null}).then((canvas) => {
            const link = document.createElement("a");
            link.href = canvas.toDataURL("image/png");
            link.download = "code-screenshot.png";
            link.click();
        });
    } else {
        console.log(valueSelected.value)
        alert('nothing to download')
        return
    }
}


let resetScreen = () => {
    code.value = ''
}

watch(code, updateHighlight);
</script>


<style>
/* Ensuring Prism styling is applied correctly */
pre code {
    font-family: 'Fira Code', monospace;
    font-size: 14px;
    white-space: pre-wrap;
}
</style>
