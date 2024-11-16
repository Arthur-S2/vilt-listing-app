<script setup>
import PaginationLinks from '../../Components/PaginationLinks.vue';
import RoleSelect from '../../Components/RoleSelect.vue';
import SessionMessages from '../../Components/SessionMessages.vue';
import InputField from '../../Components/InputField.vue';
import { router, useForm } from '@inertiajs/vue3';

defineProps({
    users: Object,
    status: String,
})
const params = route().params
const form = useForm({ search: params.search })
const search = () => {
    router.get(route('admin.index'), {
        search: form.search,
        user_role: params.user_role
    })
}
const toggleRole = (e) => {
    if (e.target.checked) {
        router.get(
            route('admin.index', {
                search: params.search,
                user_role: 'suspended'
            })
        );
    } else {
        router.get(
            route('admin.index', {
                search: params.search,
                user_role: null
            })
        );
    }
}
</script>

<template>

    <Head title="- Admin" />
    <SessionMessages :status="status" />
    <!-- Heading -->
    <div class="flex items-end justify-between mb-4">
        <div class="flex items-end gap-2">
            <!-- Search form -->
            <form @submit.prevent="search">
                <InputField label="" icon="magnifying-glass" v-model="form.search" placeholder="Search..." />
            </form>
            <Link v-if="params.search" :href="route('admin.index', { ...params, search: null, page: null })"
                class="flex items-center gap-2 px-2 py-[6px] text-white bg-indigo-500 rounded-md ">{{ params.search }}
            <i class="fa-solid fa-xmark"></i>
            </Link>
        </div>
        <!-- Toggle role btn -->
        <div class="flex items-center gap-1 px-2 py-1 text-xs rounded-md hover:bg-slate-300 dark:hover:bg-slate-800">
            <input :checked="params.user_role" @input="toggleRole" type="checkbox" id="toggleRole"
                class="text-indigo-500 rounded-md cursor-pointer border-1 outline-0 ring-indigo-500 border-slate-700">
            <label for="toggleRole" class="block text-sm font-medium cursor-pointer text-slate-700 dark:text-slate-300">
                Show suspended users
            </label>
        </div>
    </div>

    <!-- Table -->
    <table class="w-full overflow-hidden bg-white rounded-lg dark:bg-slate-800 ring-1 ring-slate-300">
        <thead>
            <tr class="text-xs text-left uppercase bg-slate-600 text-slate-300">
                <th class="w-3/6 p-3">Name</th>
                <th class="w-2/6 p-3">Role</th>
                <th class="w-1/6 p-3">Listings</th>
                <th class="w-1/6 p-3 text-right">View</th>
            </tr>
        </thead>
        <tbody class="divide-y divide-slate-300 divide-dashed">
            <tr v-for="user in users.data" :key="user.id">
                <td class="w-3/6 px-3 py-5">
                    <p class="mb-1 font-bold">{{ user.name }}</p>
                    <p class="text-xs font-light">{{ user.email }}</p>
                </td>
                <td class="w-2/6 px-3 py-5">
                    <RoleSelect :user="user" />
                </td>
                <td class="w-1/6 px-3 py-5">
                    <div class="flex items-center gap-6">
                        <div class="flex items-center gap-1">
                            <p>{{ user.listings.filter(l => l.approved).length }}</p>
                            <i class="text-green-400 fa-solid fa-circle-check"></i>
                        </div>
                        <div class="flex items-center gap-1">
                            <p>{{ user.listings.filter(l => !l.approved).length }}</p>
                            <i class="text-red-400 fa-solid fa-circle-xmark"></i>
                        </div>
                    </div>
                </td>
                <td class="w-1/6 px-3 py-5 text-right">
                    <Link :href="route('user.show', user.id)"
                        class="px-3 text-indigo-400 fa-solid fa-up-right-from-square">
                    </Link>
                </td>
            </tr>
        </tbody>
    </table>
    <div class="mt-6">
        <PaginationLinks :paginator="users" />
    </div>
</template>
