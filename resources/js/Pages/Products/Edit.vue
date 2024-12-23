<script setup>
import CurrencyInput from "@/Components/CurrencyInput.vue";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import TextAreaInput from "@/Components/TextAreaInput.vue";
import TextInput from "@/Components/TextInput.vue";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { useForm, Head } from "@inertiajs/vue3";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import { ref } from "vue";

const props = defineProps({
    product: Object,
});


const isSubmitting = ref(false);

const form = useForm({
    name: props.product.name,
    description: props.product.description,
    price: props.product.price,
});

const submit = () => {
    form.put(route("product.update", props.product.id), {});
};
</script>

<template>
    <Head title="Edit" />
    <AuthenticatedLayout>
        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8 space-y-6">
                <div
                    class="overflow-hidden bg-white shadow-sm sm:rounded-lg dark:bg-gray-800"
                >
                    <div class="p-6 text-gray-900 dark:text-gray-100">
                        <form @submit.prevent="submit">
                            <div class="mb-4">
                                <InputLabel for="nome" value="Nome" />

                                <TextInput
                                    id="nome"
                                    type="text"
                                    class="mt-1 block w-full"
                                    v-model="form.name"
                                    required
                                    autofocus
                                />

                                <InputError
                                    class="mt-2"
                                    :message="form.errors.name"
                                />
                            </div>
                            <div class="mb-4">
                                <InputLabel for="descricao" value="Descricao" />

                                <TextAreaInput
                                    id="descricao"
                                    class="mt-1 block w-full"
                                    v-model="form.description"
                                    required
                                />

                                <InputError
                                    class="mt-2"
                                    :message="form.errors.description"
                                />
                            </div>
                            <div class="mb-4">
                                <InputLabel for="preco" value="Preco" />

                                <CurrencyInput
                                    id="preco"
                                    class="mt-1 block w-full"
                                    v-model="form.price"
                                    required
                                />

                                <InputError
                                    class="mt-2"
                                    :message="form.errors.price"
                                />
                            </div>
                            <div>
                                <PrimaryButton
                                    type="submit"
                                    :disabled="isSubmitting"
                                >
                                    Atualizar Produto
                                </PrimaryButton>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
