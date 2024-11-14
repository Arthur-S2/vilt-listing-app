<script setup>
defineProps({
    paginator: Object
})
const makeLabel = (label) => {
    if (label.includes("Previous")) {
        return '<<';
    } else if (label.includes("Next")) {
        return '>>';
    } else {
        return label;
    }
}
</script>
<template>
    <div class="flex items-start justify-between">
        <div class="flex items-center overflow-hidden rounded-md shadow-lg">
            <div v-for="(link, i) in paginator.links" :key="i">
                <component :is="link.url ? 'Link' : 'span'" :href="link.url" v-html="makeLabel(link.label)" :class="{
                    'hover:bg-slate-300 dark:hover:bg-slate-500': link.url,
                    'text-slate-300': !link.url,
                    'font-bold text-indigo-500 dark:text-indigo-400': link.active,
                }"
                    class="grid w-12 h-12 bg-white border-x border-slate-50 place-items-center dark:bg-slate-900 dark:border-slate-800" />
            </div>
        </div>
        <p class="text-sm text-slate-600 dark:text-slate-400">
            Showing {{ paginator.from }} to {{ paginator.to }} of {{ paginator.total }} results
        </p>
    </div>
</template>
