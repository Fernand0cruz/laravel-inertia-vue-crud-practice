<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import { defineProps, onMounted, ref, toRefs } from "vue";
import InputError from "@/Components/InputError.vue";
import { toast } from "vue3-toastify";
import InputLabel from "@/Components/InputLabel.vue";
import TextInput from "@/Components/TextInput.vue";
import TextAreaInput from "@/Components/TextAreaInput.vue";
import CurrencyInput from "@/Components/CurrencyInput.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import DangerButton from "@/Components/DangerButton.vue";
import ConfirmModal from "@/Components/ConfirmModal.vue";

const props = defineProps({
    products: Array,
    flash: Object,
});

const { flash } = toRefs(props);
const isSubmitting = ref(false);
const isModalVisible = ref(false);
const productToDelete = ref(null);

const form = useForm({
    name: "",
    description: "",
    price: "",
});

onMounted(() => {
    if (flash.value?.message) {
        toast.success(flash.value.message, {
            theme: "auto",
        });
    }
});

const submit = () => {
    isSubmitting.value = true;

    form.post(route("product.store"), {
        onSuccess: () => {
            form.reset();
            toast.success(flash.value?.message, {
                theme: "auto",
            });
            isSubmitting.value = false;
        },
        onError: (erro) => {
            console.error(erro);
            isSubmitting.value = false;
        },
    });
};

const formatPrice = (price) => {
    const formatter = new Intl.NumberFormat("pt-BR", {
        style: "currency",
        currency: "BRL",
        minimumFractionDigits: 2,
        maximumFractionDigits: 2,
    });
    return formatter.format(price);
};

const openDeleteModal = (product) => {
    productToDelete.value = product;
    isModalVisible.value = true;
};

const confirmDelete = () => {
    if (productToDelete.value) {
        form.delete(route("product.destroy", productToDelete.value.id), {
            onSuccess: () => {
                form.reset();
                toast.success(flash.value?.message, {
                    theme: "auto",
                });
                isSubmitting.value = false;
            },
            onError: (erro) => {
                console.error(erro);
                isSubmitting.value = false;
            },
        });
    }
    isModalVisible.value = false;
};

const cancelDelete = () => {
    productToDelete.value = null;
    isModalVisible.value = false;
};
</script>

<template>
    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8 space-y-6">
                <div
                    class="overflow-hidden bg-white shadow-sm sm:rounded-lg dark:bg-gray-800"
                >
                    <div class="p-6 text-gray-900 dark:text-gray-100">
                        <h2 class="text-lg font-semibold">Criar Produto</h2>
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
                                    autocomplete="name"
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
                                    Criar Produto
                                </PrimaryButton>
                            </div>
                        </form>
                    </div>
                </div>

                <div
                    class="overflow-hidden bg-white shadow-sm sm:rounded-lg dark:bg-gray-800"
                >
                    <div class="p-6 text-gray-900 dark:text-gray-100">
                        <h2 class="text-lg font-semibold">Products</h2>
                        <table
                            class="w-full border"
                            style="table-layout: fixed"
                        >
                            <thead>
                                <tr class="dark:bg-gray-900">
                                    <th class="border p-2">nome</th>
                                    <th class="border p-2">descricao</th>
                                    <th class="border p-2">preco</th>
                                    <th class="border p-2">acao</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr
                                    v-for="(product, index) in products"
                                    :key="index"
                                    class="dark:hover:bg-gray-900"
                                >
                                    <td class="border p-2">
                                        {{ product.name }}
                                    </td>
                                    <td class="border p-2">
                                        {{ product.description }}
                                    </td>
                                    <td class="border p-2">
                                        {{ formatPrice(product.price) }}
                                    </td>
                                    <td
                                        class="border p-2 flex gap-3 justify-center"
                                    >
                                        <Link
                                            class="inline-flex items-center rounded-md border border-transparent bg-gray-800 px-4 py-2 text-xs font-semibold uppercase tracking-widest text-white transition duration-150 ease-in-out hover:bg-gray-700 focus:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 active:bg-gray-900 dark:bg-gray-200 dark:text-gray-800 dark:hover:bg-white dark:focus:bg-white dark:focus:ring-offset-gray-800 dark:active:bg-gray-300"
                                            :href="
                                                route(
                                                    'product.show',
                                                    product.id
                                                )
                                            "
                                            >Ver</Link
                                        >
                                        <Link
                                            class="inline-flex items-center rounded-md border border-transparent bg-gray-800 px-4 py-2 text-xs font-semibold uppercase tracking-widest text-white transition duration-150 ease-in-out hover:bg-gray-700 focus:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 active:bg-gray-900 dark:bg-gray-200 dark:text-gray-800 dark:hover:bg-white dark:focus:bg-white dark:focus:ring-offset-gray-800 dark:active:bg-gray-300"
                                            :href="
                                                route(
                                                    'product.edit',
                                                    product.id
                                                )
                                            "
                                            >Editar</Link
                                        >
                                        <DangerButton
                                            @click="openDeleteModal(product)"
                                            >Apagar</DangerButton
                                        >
                                    </td>
                                </tr>
                                <tr v-if="products < 1">
                                    <td
                                        class="border p-2 text-center"
                                        colspan="4"
                                    >
                                        Nenhum produto encontrado
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                        <ConfirmModal
                            :isVisible="isModalVisible"
                            title="Confirmar Exclusão"
                            message="Você tem certeza que deseja apagar este produto? Esta ação não pode ser desfeita."
                            @confirm="confirmDelete"
                            @cancel="cancelDelete"
                        />
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
