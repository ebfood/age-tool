<script setup lang="ts">
defineProps<{ title: string; data: number[]; darker?: boolean }>();
</script>

<template>
    <table class="border-collapse w-24 border border-stone-400 dark:border-stone-500 shadow-sm">
        <thead>
            <tr>
                <th class="w-1/2 border border-stone-600 font-semibold p-4 text-stone-100">
                    {{ title }}
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="num in data">
                <td
                    class="border flex justify-center border-stone-600 font-bold p-1 text-stone-100"
                    :class="{ '!text-stone-400': darker }"
                >
                    {{ num }}
                </td>
            </tr>
        </tbody>
    </table>
</template>
