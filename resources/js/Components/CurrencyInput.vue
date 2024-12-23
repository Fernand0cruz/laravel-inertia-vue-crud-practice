<script setup>
import { ref, onMounted, defineProps, defineEmits, defineExpose } from "vue";

const model = defineProps({
  modelValue: {
    type: [String, Number],
    required: true,
  },
});

const emit = defineEmits(['update:modelValue']);

const input = ref(null);

onMounted(() => {
  if (input.value.hasAttribute("autofocus")) {
    input.value.focus();
  }
});

defineExpose({
  focus: () => input.value.focus(),
});

const onInput = (event) => {
  let value = event.target.value;
  const numericValue = value.replace(/[^\d,]/g, '');
  const formattedValue = numericValue.replace(',', '.');
  emit('update:modelValue', parseFloat(formattedValue));
};
</script>

<template>
  <input
    class="rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 dark:border-gray-700 dark:bg-gray-900 dark:text-gray-300 dark:focus:border-indigo-600 dark:focus:ring-indigo-600"
    v-bind="model"
    ref="input"
    :value="model.modelValue"
    @input="onInput"
    v-mask="[
        'R$ #,##',
        'R$ ##,##',
        'R$ ###,##',
        'R$ #.###,##',
    ]"  
    placeholder="R$ 0,00"
    autocomplete="off"
  />
</template>
