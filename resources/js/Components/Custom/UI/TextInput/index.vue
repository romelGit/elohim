<template>
    <div class="mb-4">
        <label :for="id" class="block text-sm font-medium text-gray-700">{{ label }}</label>
        <input
            :id="id"
            :type="type"
            :value="modelValue"  
            @input="updateValue" 
            class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
            :required="required"
        />
    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
    modelValue: {
        type: String,
        default: ''
    },
    id: {
        type: String,
        required: true
    },
    label: {
        type: String,
        required: true
    },
    type: {
        type: String,
        default: 'text'
    },
    required: {
        type: Boolean,
        default: false
    }
});

const emit = defineEmits(['update:modelValue']);

const updateValue = (event) => {
    emit('update:modelValue', event.target.value);
};
</script>
