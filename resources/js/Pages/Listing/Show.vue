<script setup>
import Container from '../../Components/Container.vue';
import { router } from '@inertiajs/vue3';
const props = defineProps({
    listing: Object,
    user: Object,
    canModify: Boolean
})
const deleteListing = () => {
    if (confirm('Are you sure?')) {
        router.delete(route('listing.destroy', props.listing.id))
    }
}

</script>
<template>

    <Head title="- Listing Detail" />
    <Container class="flex gap-4">
        <div class="w-1/4 overflow-hidden rounded-md">
            <img class="object-cover object-center w-full h-full"
                :src="listing.image ? `/storage/${listing.image}` : '/storage/images/listing/default.png'" alt="">
        </div>
        <div class="w-3/4">
            <!-- Listing info -->
            <div class="mb-6">
                <div class="flex items-end justify-between mb-2">
                    <p class="w-full border-b text-slate-400">
                        Listing detail
                    </p>

                    <!-- Edit and delete buttons -->
                    <div v-if="canModify" class="flex items-center gap-4 pl-4">

                        <Link :href="route('listing.edit', listing.id)"
                            class="px-6 py-2 text-white bg-green-500 rounded-md hover:outline outline-green-500 outline-offset-2">
                        Edit
                        </Link>
                        <button @click="deleteListing"
                            class="px-6 py-2 text-white bg-red-500 rounded-md hover:outline outline-red-500 outline-offset-2">
                            Delete
                        </button>
                    </div>
                </div>

                <h3 class="mb-4 text-2xl font-bold">{{ listing.title }}</h3>
                <p>{{ listing.desc }}</p>

            </div>

            <!-- Contact info -->
            <div class="mb-6">
                <p class="w-full mb-2 border-b text-slate-400">
                    Contact info
                </p>
                <!-- Email -->
                <div v-if="listing.email" class="flex items-center gap-2 mb-2">
                    <i class="fa-solid fa-at"></i>
                    <p>Email:</p>
                    <a :href="`mailto:${listing.email}`" class="text-link">
                        {{ listing.email }}
                    </a>
                </div>
                <!-- Link -->
                <div v-if="listing.link" class="flex items-center gap-2 mb-2">
                    <i class="fa-solid fa-up-right-from-square"></i>
                    <p>External Link:</p>
                    <a :href="listing.link" target="_blank" class="text-link">
                        {{ listing.link }}
                    </a>
                </div>
                <!-- User -->
                <div class="flex items-center gap-2 mb-2">
                    <i class="fa-solid fa-user"></i>
                    <p>Listed by:</p>
                    <Link :href="route('home', { user_id: user.id })" class="text-link">
                    {{ user.name }}
                    </Link>
                </div>
            </div>

            <!-- Tags -->
            <div v-if="listing.tags" class="mb-6">
                <p class="w-full mb-2 border-b text-slate-400">Tags</p>

                <div class="flex items-center gap-3">
                    <div v-for="tag in listing.tags.split(',')" :key="tag">
                        <Link :href="route('home', { tag })"
                            class="px-2 py-px text-white rounded-full bg-slate-500 hover:bg-slate-700 dark:hover:bg-slate-900">
                        {{ tag }}
                        </Link>
                    </div>
                </div>
            </div>
        </div>
    </Container>
</template>
