<template>
    <div
        class="flex flex-col items-start justify-start max-w-screen md:flw-row relative py-4 p-4 bg-gray-900 h-screen overflow-x-hidden">
        <div
            class=" flex flex-col w-3xl md:flex-row md:w-screen  rounded-lg shadow-lg h-screen">
            <!-- Input Area -->
            <textarea
                v-model="code"
                placeholder="Paste your code here..."
                class="w-96 pt-8 h-1/2 py-12 md:h-screen p-3 text-white bg-gray-800 border border-gray-700 rounded-lg font-mono focus:outline-none"
            ></textarea>

            <!-- Code Preview with Highlighting -->
            <div class="w-fit">
                <SelectLanguage
                    @languageChanged="handleLanguageSelection"
                />
                <pre
                    ref="codeContainer"
                    class="rounded-lg w-full h-screen md:h-fit ml-2 flex flex-col bg-gray-900 px-6 pt-6">
                    <TitleBar
                        :languageName="languageName"
                    />
                <code :class="[language, 'max-w-6xl w-fit bg-gray-900 py-12 h-full h-fit bg-red-600']"
                      v-html="Prism.highlight(code, Prism.languages.css, codelang)"></code>
            </pre>
            </div>
        </div>
        <div

            class="flex  fixed top-2 right-0 md:right-1/3 w-full justify-between md:justify-end">
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
import SelectLanguage from "@/Components/SelectLanguage.vue";

const code = ref('');
let codeContainer = ref('');

let codelang = ref('')
let language = ref('language-' + codelang)
let valueSelected = ref();


let prism = ref(Prism.languages.css)
const updateHighlight = () => {
    setTimeout(() => {
        Prism.highlightAll();
    }, 100);
};

let languageName = ref('')

const handleLanguageSelection = (selectedValue) => {
    language.value = 'language-' + selectedValue.alias;
    codelang.value = selectedValue.alias
    valueSelected.value = selectedValue.alias;
    prism.value = 'Prism.languages.' + codelang.value
    languageName.value = selectedValue.name
    updateHighlight()
};
const saveAsImage = () => {
    if (code.value !== '' && valueSelected.value !== '') {
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
