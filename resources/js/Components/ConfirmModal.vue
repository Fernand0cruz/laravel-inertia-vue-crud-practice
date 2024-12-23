<script setup>
import { defineProps, defineEmits } from "vue";
import PrimaryButton from "./PrimaryButton.vue";
import DangerButton from "./DangerButton.vue";

const props = defineProps({
    isVisible: Boolean,
    title: String,
    message: String,
});

const emit = defineEmits(["confirm", "cancel"]);

const onConfirm = () => emit("confirm");
const onCancel = () => emit("cancel");
</script>

<template>
    <div
        v-if="isVisible"
        class="fixed inset-0 z-50 flex items-center justify-center bg-gray-900 bg-opacity-75"
    >
        <div
            class="bg-white dark:bg-gray-900 border rounded-lg p-6 shadow-lg max-w-md w-full"
        >
            <h2 class="text-lg font-semibold mb-4">{{ title }}</h2>
            <p class="text-gray-900 dark:text-gray-100 mb-6">{{ message }}</p>
            <div class="flex justify-end space-x-4">
                <PrimaryButton @click="onCancel"> Cancelar </PrimaryButton>
                <DangerButton @click="onConfirm"> Confirmar </DangerButton>
            </div>
        </div>
    </div>
</template>
