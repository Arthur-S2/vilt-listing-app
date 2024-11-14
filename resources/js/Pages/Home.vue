<script setup>
import Card from '../Components/Card.vue';
import PaginationLinks from '../Components/PaginationLinks.vue';
import InputField from '../Components/InputField.vue';
import { router, useForm } from '@inertiajs/vue3';

const params = route().params;

const props = defineProps({
    listings: Object,
    searchTerm: String,
});
const username = params.user_id ? props.listings.data.find(i => i.user_id === Number(params.user_id)).user.name : null

const form = useForm({
    search: props.searchTerm,
});
const search = () => {
    router.get(route('home'), { search: form.search, user_id: params.user_id, tag: params.tag });
}
</script>

<template>

    <Head title="- Latest Listings" />

    <div class="flex items-center justify-between mb-4">
        <div class="flex items-center gap-2">
            <Link v-if="params.tag" :href="route('home', { ...params, tag: null, page: null })"
                class="flex items-center gap-2 px-2 py-1 text-white bg-indigo-500 rounded-md ">{{ params.tag }}
            <i class="fa-solid fa-xmark"></i>
            </Link>
            <Link v-if="params.search" :href="route('home', { ...params, search: null, page: null })"
                class="flex items-center gap-2 px-2 py-1 text-white bg-indigo-500 rounded-md ">{{ params.search }}
            <i class="fa-solid fa-xmark"></i>
            </Link>
            <Link v-if="params.user_id" :href="route('home', { ...params, user_id: null, page: null })"
                class="flex items-center gap-2 px-2 py-1 text-white bg-indigo-500 rounded-md ">{{ username }}
            <i class="fa-solid fa-xmark"></i>
            </Link>
        </div>

        <div class="w-1/4">
            <form @submit.prevent="search">
                <InputField v-model="form.search" label="" type="search" icon="magnifying-glass"
                    placeholder="Search..." />
            </form>
        </div>

    </div>

    <div v-if="Object.keys(listings.data).length">
        <div class="grid grid-cols-3 gap-4">
            <div v-for="listing in listings.data" :key="listing.id">
                <Card :listing="listing" />
            </div>
        </div>
        <div class="mt-8">
            <PaginationLinks :paginator="listings" />
        </div>
    </div>
    <div v-else>
        There are no listings
    </div>
</template>
