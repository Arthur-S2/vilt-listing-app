<script setup>
import Container from '../../../Components/Container.vue';
import Title from '../../../Components/Title.vue';
import InputField from '../../../Components/InputField.vue';
import PrimaryBtn from '../../../Components/PrimaryBtn.vue';
import ErrorMessages from '../../../Components/ErrorMessages.vue';
import SessionMessages from '../../../Components/SessionMessages.vue';
import { useForm } from '@inertiajs/vue3';

const form = useForm({
    current_password: '',
    password: '',
    password_confirmation: ""
})
const submit = () => {
    form.put(route('profile.password'), {
        preserveScroll: true,
        onSuccess: () => form.reset(),
        onError: () => form.reset()
    });
}

</script>
<template>
    <Container class="mb-6">
        <div class="mb-6">
            <Title>Update Password</Title>
            <p>Ensure you are using a long, random password to stay secure.</p>
        </div>

        <ErrorMessages :errors="form.errors" />

        <form class="space-y-6" @submit.prevent="submit">
            <InputField label="Current Password" icon="key" class="w-1/2" v-model="form.current_password"
                type="password" />
            <InputField label="New Password" icon="key" class="w-1/2" v-model="form.password" type="password" />
            <InputField label="Confirm New Password" icon="key" class="w-1/2" v-model="form.password_confirmation"
                type="password" />

            <p v-if="form.recentlySuccessful" class="font-medium text-green-500">Saved!</p>

            <PrimaryBtn :disabled="form.processing">Save</PrimaryBtn>
        </form>
    </Container>
</template>
