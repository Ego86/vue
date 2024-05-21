<template>
    <div>
      <select v-model="selectedValue">
        <option disabled value="">Выберите из списка</option>
        <option v-for="option in options" :key="option.value" :value="option.value">
          {{ option.name }}
        </option>
      </select>
      <button @click="submitOption">Выполнить</button>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { defineProps, defineEmits } from 'vue';
  
  const props = defineProps({
    modelValue: {
      type: String,
      required: true
    },
    options: {
      type: Array,
      default: () => []
    }
  });
  
  const emit = defineEmits(['update:modelValue']);
  
  const selectedValue = ref(props.modelValue);
  
  const submitOption = () => {
    emit('update:modelValue', selectedValue.value);
  };
  </script>
  
  <style>

  </style>
  