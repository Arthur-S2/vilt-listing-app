<script setup>
import PaginationLinks from '../../Components/PaginationLinks.vue';


defineProps({
    users: Object
})
</script>

<template>

    <Head title="- Admin" />
    <!-- Heading -->
    <div>
        Heading
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
                    {{ user.role }}
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
                    View link
                </td>
            </tr>
        </tbody>
    </table>
    <div class="mt-6">
        <PaginationLinks :paginator="users" />
    </div>
</template>
