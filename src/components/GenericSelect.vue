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
  placeholder: {
    type: String,
    default: 'Choose...'
  },
  clearable: {
    type: Boolean,
    default: false
  },
  isModified: {
    type: Boolean,
    default: false
  }
})

const labelSuffix = ' (modified)'
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
    <template #selected-option="option">
      <span style="white-space: pre">
        {{ option[labelKey] }}<span v-if="isModified">{{ labelSuffix }}</span>
      </span>
    </template>
    <template #option="option">
      <div>
        <h3 style="margin: 0; color: black">{{ option[labelKey] }}</h3>
        <em v-if="option.properties" style="margin: 0; color: #7a7a7a">
          <template v-for="(property, key, idx) in option.properties" :key="key">
            {{ key }}: {{ property.defaultValue }} {{ property.units }}
            <span v-if="idx < Object.keys(option.properties).length - 1">, </span>
          </template>
        </em>
      </div>
    </template>
  </v-select>
</template>

<style scoped>
:deep() {
  --vs-controls-color: #e7e7e7;
  --vs-border-color: #10b98150;

  --vs-dropdown-bg: #e7e7e7;
  --vs-dropdown-color: #333333;

  --vs-dropdown-option--active-bg: #10b981;
  --vs-dropdown-option--active-color: #333333;

  --vs-indicator-color: #d9d9d9;
}

.v-select {
  margin-bottom: 1rem;
  border: 1px solid var(--vs-border-color);
  border-radius: 6px;
  background-color: var(--vs-controls-color);
}

:deep() {
  .vs__dropdown-menu {
    background-color: var(--vs-dropdown-bg);
    color: var(--vs-dropdown-color);
    border-radius: 6px;
  }

  .vs__dropdown-option {
    color: var(--vs-dropdown-color);
    padding: 8px 12px;
  }

  .vs__open-indicator {
    fill: var(--vs-indicator-color);
    background: var(--vs-dropdown-bg);
  }
}
</style>
