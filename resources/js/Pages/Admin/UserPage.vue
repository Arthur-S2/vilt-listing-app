<script setup>
import { router, useForm } from '@inertiajs/vue3';
import Title from '../../Components/Title.vue';
import InputField from '../../Components/InputField.vue';
import PaginationLinks from '../../Components/PaginationLinks.vue';
import SessionMessages from '../../Components/SessionMessages.vue';
const props = defineProps({
    user: Object,
    listings: Object,
    status: String
});
const params = route().params
const form = useForm({ search: params.search })
const search = () => {
    router.get(route('user.show', {
        user: props.user.id,
        search: form.search,
        disapproved: params.disapproved
    }))
}
const showDisapproved = (e) => {
    if (e.target.checked) {
        router.get(
            route('user.show', {
                user: props.user.id,
                search: params.search,
                disapproved: true,
            })
        );
    } else {
        router.get(
            route('user.show', {
                user: props.user.id,
                search: params.search,
                disapproved: null,
            })
        );
    }
}
const toggleApprove = (listing) => {
    let msg = listing.approved ? 'Disapprove this listing?' : 'Approve this listing?'
    if (confirm(msg)) {
        router.put(route('admin.approve', listing.id))
    }
}
</script>
<template>

    <Head :title="`- ${user.name} Listings`"></Head>
    <!-- Heading -->
    <div class="mb-6">
        <Title>{{ user.name }} latest listings</Title>
        <SessionMessages :status="status" />
        <div class="flex items-end justify-between">
            <div class="flex items-end gap-2">
                <!-- Search form -->
                <form @submit.prevent="search">
                    <InputField label="" icon="magnifying-glass" v-model="form.search" placeholder="Search..." />
                </form>
                <Link v-if="params.search"
                    :href="route('user.show', { ...params, search: null, page: null, user: user.id })"
                    class="flex items-center gap-2 px-2 py-[6px] text-white bg-indigo-500 rounded-md ">{{ params.search
                }}
                <i class="fa-solid fa-xmark"></i>
                </Link>
            </div>
            <div
                class="flex items-center gap-1 px-2 py-1 text-xs rounded-md hover:bg-slate-300 dark:hover:bg-slate-800">
                <input :checked="params.disapproved" @input="showDisapproved" type="checkbox" id="showDisapproved"
                    class="text-indigo-500 rounded-md cursor-pointer border-1 outline-0 ring-indigo-500 border-slate-700">
                <label for="showDisapproved"
                    class="block text-sm font-medium cursor-pointer text-slate-700 dark:text-slate-300">
                    Show disapproved listings
                </label>
            </div>
        </div>
    </div>
    <!-- Table -->
    <table class="w-full overflow-hidden bg-white rounded-lg dark:bg-slate-800 ring-1 ring-slate-300">
        <thead>
            <tr class="text-xs text-left uppercase bg-slate-600 text-slate-300">
                <th class="w-4/6 p-3">Title</th>
                <th class="w-2/6 p-3 text-center">Approved</th>
                <th class="w-1/6 p-3 text-right">View</th>
            </tr>
        </thead>
        <tbody class="divide-y divide-slate-300 divide-dashed">
            <tr v-for="listing in listings.data" :key="listing.id">
                <td class="px-3 py-5">{{ listing.title }}</td>
                <td class="px-3 py-5 text-2xl text-center">
                    <button @click.prevent="toggleApprove(listing)">
                        <i
                            :class="`fa-solid fa-${listing.approved ? 'circle-check text-green-400' : 'circle-xmark text-red-400'}`"></i>
                    </button>
                </td>
                <td class="w-1/6 px-6 py-5 text-right">
                    <Link class="px-3 text-indigo-400 fa-solid fa-up-right-from-square"
                        :href="route('listing.show', listing.id)">
                    </Link>
                </td>
            </tr>
        </tbody>
    </table>
    <div class="mt-6">
        <PaginationLinks :paginator="listings" />
    </div>
</template>
