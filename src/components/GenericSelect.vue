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
    default: 'Choose...'
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
:deep() {
  --vs-controls-color: #d9d9d9;
  --vs-border-color: #10b98150;

  --vs-dropdown-bg: #d9d9d9;
  --vs-dropdown-color: #333333;

  --vs-selected-bg: #10b981;
  --vs-selected-color: #333333;

  --vs-search-input-color: #333333;

  --vs-dropdown-option--active-bg: #10b981;
  --vs-dropdown-option--active-color: #333333;
}

.v-select {
  margin-bottom: 1rem;
  border: 1px solid var(--vs-border-color);
  border-radius: 6px;
  background-color: var(--vs-controls-color);
}

.v-select.vs--single .vs__selected {
  color: var(--vs-selected-color);
  font-weight: normal;
}

.vs__dropdown-menu {
  background-color: var(--vs-dropdown-bg);
  color: var(--vs-dropdown-color);
  border-radius: 6px;
}

.vs__dropdown-option {
  color: var(--vs-dropdown-color);
  padding: 8px 12px;
}

.vs__dropdown-option:hover,
.vs__dropdown-option--active {
  background-color: var(--vs-dropdown-option--active-bg);
  color: var(--vs-dropdown-option--active-color);
}

.vs__placeholder {
  color: var(--vs-dropdown-color);
  opacity: 0.7;
}

</style>
