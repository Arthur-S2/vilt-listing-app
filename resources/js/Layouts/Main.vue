<script setup>
import { switchTheme } from '../theme';
import NavLink from '../Components/NavLink.vue';
import { computed, ref } from 'vue';
import { Link, usePage } from '@inertiajs/vue3';

const page = usePage()
const user = computed(() => page.props.auth.user)

const show = ref(false);

</script>

<template>
    <!------Overlay------>
    <div v-show="show" @click="show = false" class="fixed inset-0 z-40"></div>

    <header class="text-white bg-slate-800">
        <nav class="flex items-center justify-between max-w-screen-lg p-6 mx-auto">
            <NavLink routeName="home" componentName="Home">Home</NavLink>
            <div class="flex items-center space-x-6">
                <!-- ----------------------Auth-------------------- -->
                <div v-if="user" class="relative">
                    <div @click='show = !show'
                        class="flex items-center gap-2 px-3 py-1 rounded-lg hover:bg-slate-700 cursor pointer"
                        :class="{ 'bg-slate-700': show }">
                        <p>{{ user.name }}</p>
                        <i class="fa-angle-down fa-solid"></i>
                    </div>
                    <!-- ----------------------User dropdown menu-------------------- -->
                    <div v-show="show" @click="show = false"
                        class="absolute right-0 z-50 w-40 overflow-hidden text-white border rounded-lg top-16 bg-slate-800 border-slate-200">
                        <Link :href="route('listing.create')"
                            class="block w-full px-6 py-3 text-left hover:bg-slate-700">
                        New Listing</Link>
                        <Link :href="route('profile.edit')" class="block w-full px-6 py-3 text-left hover:bg-slate-700">
                        Profile</Link>
                        <Link :href="route('dashboard')" class="block w-full px-6 py-3 text-left hover:bg-slate-700">
                        Dashboard</Link>
                        <Link as="button" method="post" :href="route('logout')"
                            class="block w-full px-6 py-3 text-left hover:bg-slate-700">Logout</Link>
                    </div>

                </div>

                <!-- ----------------------Guest------------------- -->
                <div v-else class="space-x-6">
                    <NavLink routeName="login" componentName="Auth/Login">Login</NavLink>
                    <NavLink routeName="register" componentName="Auth/Register">Register</NavLink>
                </div>
                <button @click="switchTheme"
                    class="grid w-6 h-6 rounded-full hover:bg-slate-700 place-items-center hover:outline outline-1 outline-white">
                    <i class="fa-solid fa-circle-half-stroke"></i>
                </button>
            </div>
        </nav>
    </header>

    <main class="p-6">
        <slot />
    </main>
</template>
