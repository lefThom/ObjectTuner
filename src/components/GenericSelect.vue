<script setup>
import { defineProps, defineEmits } from 'vue'
import vSelect from 'vue-select'
import 'vue-select/dist/vue-select.css'

const props = defineProps({
  options: Array,
  modelValue: Number, // l'Id de l'object selectionné
  valueKey: {
    type: String,
    default: 'id'
  },
  labelKey: {
    type: String,
    default: 'name'
  },
  displayKeys: {
    type: Array,
    default: () => []
  },
  placeholder: {
    type: String,
    default: 'Choisir...'
  },
  clearable: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['update:modelValue'])
</script>

<template>
  <v-select
    :options="options"
    :label="labelKey"
    :reduce="opt => opt.id"
    :clearable="clearable"
    :placeholder="placeholder"
    :modelValue="modelValue"
    @update:modelValue="val => emit('update:modelValue', val)"
  >
    <template #option="option">
      <div>
        <h3 style="margin: 0; color: black">{{ option[labelKey] }}</h3>
        <em v-if="displayKeys.length" style="margin: 0; color: #7a7a7a">
          <template v-for="(key, idx) in displayKeys" :key="key">
            {{ key }}: {{ option[key] }}<span v-if="idx < displayKeys.length - 1">, </span>
          </template>
        </em>
      </div>
    </template>
  </v-select>
</template>

<style scoped>
  .v-select {
   margin-bottom: 1rem;
  }

  :deep() {
    --vs-controls-color: #d9d9d9;
    --vs-border-color: #10b98150;

    --vs-dropdown-bg: #d9d9d9;
    --vs-dropdown-color: #cc99cd;
    --vs-dropdown-option-color: #cc99cd;

    --vs-selected-bg: #10b981;
    --vs-selected-color: #eeeeee;

    --vs-search-input-color: #eeeeee;

    --vs-dropdown-option--active-bg: #10b981;
    --vs-dropdown-option--active-color: #eeeeee;
  }
</style>
