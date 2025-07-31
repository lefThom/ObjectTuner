<script setup>
import { ref, computed, onMounted } from 'vue'
import SingleTuner from './SingleTuner.vue'
import GenericSelect from './GenericSelect.vue'

// Récupération du type de boîte depuis les props
const props = defineProps(['boxTitle'])

const objects = ref([])
const selectedId = ref(null)

const selectedObject = computed(() =>
  objects.value.find((obj) => obj.id === selectedId.value)
)

onMounted(async () => {
  const res = await fetch('/data.json')
  objects.value = await res.json()
  if (objects.value.length) {
    selectedId.value = objects.value[0].id
  }
})
</script>

<template>
  <div class="ObjectTunerBox" v-if="objects.length">
    <h2>{{ boxTitle }}</h2>
    <GenericSelect
      v-model="selectedId"
      :options="objects"
      :labelKey="'name'"
      :clearable="false"
    />

    <div v-if="selectedObject">
      <SingleTuner
        v-for="(prop, key) in selectedObject.properties"
          :key="`prop-${selectedObject.id}-${key}`"
          :propertyName="key"
          :value="prop.defaultValue"
          :min="prop.min"
          :max="prop.max"
          :step="prop.step"
          :units="prop.units"
          @update:value="val => prop.value = val"
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
