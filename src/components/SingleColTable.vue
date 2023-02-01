<script setup lang="ts">
defineProps<{ title: string; data: number[]; darker?: boolean }>();
</script>

<template>
    <table
        class="border-collapse w-24 border border-stone-400 dark:border-stone-500 bg-white dark:bg-stone-800 shadow-sm"
    >
        <thead class="bg-stone-50 dark:bg-stone-700">
            <tr>
                <th class="w-1/2 border border-stone-300 dark:border-stone-600 font-semibold p-4 text-stone-200">
                    {{ title }}
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="num in data">
                <td
                    class="border flex justify-center border-stone-300 dark:border-stone-700 font-bold p-1 text-stone-200"
                    :class="{ 'text-stone-400': darker }"
                >
                    {{ num }}
                </td>
            </tr>
        </tbody>
    </table>
</template>
