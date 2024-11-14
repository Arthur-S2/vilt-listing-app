<script setup>
import { ref } from 'vue';
const emit = defineEmits(['image'])

const props = defineProps({
    listingImage: String,
})
const currentImage = props.listingImage ? `/storage/${props.listingImage}` : null;
const preview = ref(currentImage);
const oversizedImage = ref(false);
const showRevertBtn = ref(false);

const imageSelected = (e) => {
    preview.value = URL.createObjectURL(e.target.files[0]);
    oversizedImage.value = e.target.files[0].size > 3145728;
    showRevertBtn.value = true;
    emit('image', e.target.files[0]);
};
const revertImageChange = () => {
    showRevertBtn.value = false;
    preview.value = currentImage;
    oversizedImage.value = false;
    emit('image', null);
}
</script>
<template>
    <div>
        <span :class="{ '!text-red-500': oversizedImage }"
            class="block text-sm font-medium text-slate-700 dark:text-slate-300">
            {{ oversizedImage ? 'The selected image exceeds 3MB' : 'Image (Max size 3MB)' }}
        </span>
        <label :class="{ ' !border-red-500': oversizedImage }" for="image"
            class="block relative h-[140px] mt-1 overflow-hidden border rounded-md cursor-pointer bg-slate-300 border-slate-300">
            <img :src="preview ?? '/storage/images/listing/default.png'"
                class="object-cover object-center w-full h-full" alt="">
            <button @click.prevent="revertImageChange" type="button" v-if="showRevertBtn"
                class="absolute grid w-8 h-8 rounded-full top-2 right-2 bg-white/75 place-items-center text-slate-700">
                <i class="fa-solid fa-rotate-left"></i>
            </button>
        </label>
        <input @input="imageSelected" type="file" name="image" id="image" hidden>
    </div>
</template>
