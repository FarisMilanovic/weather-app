<template>
    <Teleport to='body'>
        <Transition name="modal-outer">
            <div v-show="modalActive"
                class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8">
                <Transition name="modal-inner">
                    <div v-if="modalActive" class="p-4 bg-white self-start mt-32 max-w-screen-md">
                        <slot></slot>
                        <button @click="$emit('close-modal')"
                            class="text-white mt-8 py-2 px-6 bg-weather-primary hover:bg-weather-secondary">Close</button>
                    </div>
                </Transition>
            </div>
        </Transition>
    </Teleport>
</template>

<script setup>
defineEmits(['close-modal']);
defineProps({
    modalActive: {
        type: Boolean,
        default: false,
    },
});
</script>

<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
    transition: opacity 0.6s ease;
}

.modal-outer-enter-from,
.modal-outer-leave-to {
    opacity: 0;
}
</style>