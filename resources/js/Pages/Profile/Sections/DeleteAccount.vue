<script setup>
import Container from '../../../Components/Container.vue';
import Title from '../../../Components/Title.vue';
import InputField from '../../../Components/InputField.vue';
import PrimaryBtn from '../../../Components/PrimaryBtn.vue';
import ErrorMessages from '../../../Components/ErrorMessages.vue';
import { useForm } from '@inertiajs/vue3';
import { ref } from 'vue';

const showConfirmPassword = ref(false);

const props = defineProps({
    //
});

const form = useForm({
    password: "",
})
const submit = () => {
    form.delete(route('profile.destroy'), {
        preserveScroll: true,
        onFinish: () => form.reset()
    })
}


</script>
<template>
    <Container class="mb-6">
        <div class="mb-6">
            <Title>Delete Account</Title>
            <p>Once your account is deleted, all of its resources data will be permanently deleted. This action cannot
                be undone.</p>
        </div>


        <ErrorMessages :errors="form.errors" />

        <div v-if="showConfirmPassword">
            <form class="flex items-end gap-4" @submit.prevent="submit">
                <InputField label="Confirm Password" icon="key" class="w-1/2" type="password" v-model="form.password" />

                <PrimaryBtn :disabled="form.processing">Confirm</PrimaryBtn>
                <button @click="showConfirmPassword = false"
                    class="font-medium text-indigo-500 underline dark:text-indigo-400">Cancel</button>

            </form>
        </div>

        <button v-if="!showConfirmPassword" @click="showConfirmPassword = true"
            class="px-6 py-2 text-white bg-red-500 rounded-lg">
            <i class="mr-2 fa-solid fa-triangle-exclamation"> </i>
            Delete Account
        </button>

    </Container>
</template>
