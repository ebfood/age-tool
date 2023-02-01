<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue';
import GeneralButton from './components/GeneralButton.vue';
import SingleColTable from './components/SingleColTable.vue';
const inputText = ref('');
const ageList = ref<number[]>([]);

const hasError = ref(false);

function onAdd() {
    const str = inputText.value.replace(/[^\d]/g, '');
    if (!str.length) {
        inputText.value = '';
        hasError.value = true;
        return;
    }
    const num = Number(str);

    if (num < 1 || num > 100) {
        inputText.value = '';
        hasError.value = true;
        return;
    }

    ageList.value.push(num);
    inputText.value = '';
}

function onDelete(index: number) {
    ageList.value.splice(index, 1);
}

function onClear() {
    ageList.value = [];
}

const showList = computed(() => [...ageList.value].reverse());

const countList = computed(() => {
    const dict: { [prop: number]: number } = {};
    for (let i = 1; i <= 100; i++) {
        dict[i] = 0;
    }

    for (const age of ageList.value) {
        if (age < 1 || age > 100) continue;
        dict[age] += 1;
    }

    return Object.values(dict);
});

watch(
    () => ageList.value.length,
    () => {
        localStorage.setItem('history', ageList.value.join(','));
    }
);

onMounted(() => {
    const history = localStorage.getItem('history');
    if (history) {
        ageList.value = history.split(',').map((item) => Number(item));
    }
});
</script>

<template>
    <div class="container mx-auto flex p-8">
        <div class="flex-1">
            <h1 class="text-3xl font-bold">输入</h1>

            <div class="flex items-center mt-4">
                <input
                    v-model="inputText"
                    @focus="hasError = false"
                    @keydown.enter="onAdd"
                    class="font-bold text-xl rounded-l h-8 px-4 bg-stone-700 border-none outline-none"
                />
                <GeneralButton @click="onAdd">添加</GeneralButton>
            </div>
            <span class="text-red-600" v-if="hasError">输入错误</span>

            <div class="mt-10">
                <div class="flex gap-8 items-center mb-8">
                    <div class="font-bold text-lg">人数：{{ ageList.length }}</div>
                    <GeneralButton @click="onClear">清空</GeneralButton>
                </div>

                <div v-for="(age, index) in showList" :key="index" class="flex items-center mt-2">
                    <div class="bg-stone-600 rounded-l-sm py-1 px-4 w-20 h-8 font-bold text-lg">{{ age }}</div>
                    <button
                        @click="onDelete(showList.length - 1 - index)"
                        class="rounded-r-sm px-4 font-bold text-lg h-8 bg-violet-500"
                    >
                        x
                    </button>
                </div>
            </div>
        </div>
        <div class="flex-1">
            <h1 class="text-3xl font-bold">输出</h1>
            <div class="flex">
                <SingleColTable :title="'年龄'" :darker="true" :data="[...Array(101).keys()].slice(1)"></SingleColTable>
                <SingleColTable :title="'人数'" :data="countList"></SingleColTable>
            </div>
        </div>
    </div>
</template>
