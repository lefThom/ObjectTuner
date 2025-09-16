<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import SingleTuner from './SingleTuner.vue'
import GenericSelect from './GenericSelect.vue'

// Props passed from the parent component
const props = defineProps(['objectTitle'])

// Reactive state for object list and currently selected object's ID
const objects = ref([])
const selectedId = ref(null)
const selectedObjectModified = ref(false)

// Computed property to retrieve the selected object based on selectedId
const selectedObject = computed(() =>
  objects.value.find((obj) => obj.id === selectedId.value)
)

// Computed property to filter out properties where display is explicitly false
// This returns an array of key + property data for rendering sliders
const filteredProperties = computed(() => {
  if (!selectedObject.value || !selectedObject.value.properties) return []

  return Object.entries(selectedObject.value.properties)
    .filter(([_, prop]) => prop && prop.display !== false)
    .map(([key, prop]) => ({ key, ...prop }))
})

// Load objects data from JSON when the component is mounted
onMounted(async () => {
  const res = await fetch('/data.json')
  objects.value = await res.json()

  // Automatically select the first object by default
  if (objects.value.length) {
    selectedId.value = objects.value[0].id
  }
})

function onPropertyUpdate(key, value) {
  console.log(`Property ${key} updated to`, value)
  selectedObjectModified.value = true;
}

function onSelectObject(val) {
  selectedId.value = val
  selectedObjectModified.value = false // reset même si c’est le même objet
}
</script>

<template>
  <div class="ObjectTunerBox" v-if="objects.length">
    <h2>{{ objectTitle }}</h2>
    <GenericSelect
      v-model="selectedId"
      :options="objects"
      :labelKey="'objectName'"
      :clearable="false"
      :isModified=selectedObjectModified
      @update:modelValue="onSelectObject"
    />

    <div v-if="selectedObject">
      <SingleTuner
        v-for="(prop, key) in filteredProperties"
          :key="`prop-${selectedObject.id}-${key}`"
          :propertyName="prop.key"
          :value="prop.defaultValue"
          :min="prop.min"
          :max="prop.max"
          :step="prop.step"
          :units="prop.units"
          @update:value="val => {
            prop.value = val
            onPropertyUpdate(prop.key, val)
          }"
      />
    </div>
  </div>
</template>

<style scoped>
  .ObjectTunerBox {
    border: 1px solid var(--color-border);
    background-color: var(--color-background-soft);
    padding: 1.5rem;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    margin: 2rem auto;
    max-width: 600px;
  }

  h2 {
    margin-bottom: 0.5rem;
  }

</style>
