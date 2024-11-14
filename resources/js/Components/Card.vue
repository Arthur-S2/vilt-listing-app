<script setup>
import { Link, router } from '@inertiajs/vue3';

defineProps({
    listing: Object
})
const params = route().params;

const selectUser = (id) => {
    router.get(route('home'), {
        user_id: id, search: params.search, tag: params.tag
    })
}

const selectTag = (tag) => {
    router.get(route('home'), {
        user_id: params.user_id,
        search: params.search,
        tag: tag
    })
}

</script>
<template>
    <div class="flex flex-col justify-between h-full overflow-hidden bg-white rounded-lg shadow-lg dark:bg-slate-800">
        <div>
            <!-- Image -->
            <Link :href="route('listing.show', listing.id)">
            <img :src="listing.image ? `/storage/${listing.image}` : '/storage/images/listing/default.png'"
                class="object-cover object-center w-full h-48 bg-slate-300" alt="">
            </Link>
            <!-- title & user -->
            <div class="p-4">
                <h3 class="mb-2 text-xl font-bold">
                    {{ listing.title.substring(0, 40) }}...
                </h3>
                <p>Listed on {{ new Date(listing.created_at).toLocaleDateString() }} by
                    <button @click="selectUser(listing.user.id)" class="text-link">{{
                        listing.user.name }}</button>
                </p>
            </div>
        </div>
        <!-- tags -->
        <div v-if="listing.tags" class="flex items-center gap-3 px-4 pb-4">
            <div v-for="tag in listing.tags.split(',')" :key="tag">
                <button @click="selectTag(tag)"
                    class="px-2 py-px text-white rounded-full bg-slate-500 hover:bg-slate-700 dark:hover:bg-slate-900">
                    {{ tag }}
                </button>
            </div>
        </div>
    </div>
</template>
