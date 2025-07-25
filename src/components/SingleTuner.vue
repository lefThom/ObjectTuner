<script setup>
import { ref, watch, toRefs } from 'vue'
import Slider from '@vueform/slider'

// Props expected from parent component
const props = defineProps({
  propertyName: String,
  value: Number,
  min: Number,
  max: Number,
  step: Number,
  units: String
})

// Utilisez toRefs pour déstructurer les props tout en conservant la réactivité
const { propertyName, value, min, max, step, units } = toRefs(props);
const sliderValue = ref(props.value)

// Define the emit event for two-way binding
const emit = defineEmits(['update:value'])

// Keep sliderValue in sync when the parent updates the value prop
watch(
  () => props.value,
  (newVal) => {
    if (newVal !== sliderValue.value) {
      sliderValue.value = newVal
    }
  },
  { immediate: true }
)

// Emit updated value when user interacts with slider/input
watch(sliderValue, (newVal) => {
  emit('update:value', newVal)
})

let shouldFormat = false // format is stopping fluid drag
</script>

<template>
  <div class="single-tuner" style="margin: 1rem 0;">
    <label :for="`${propertyName}-input`"> {{ propertyName }} </label>
    <Slider class="Slider"
      v-model="sliderValue"
      :min="min"
      :max="max"
      :step="step"
      :lazy="false"
      :format="shouldFormat ? val => `${val} ${units}` : null"
      show-tooltip="drag"
    /> <!-- format is stopping fluid drag -->
    <input
      :id="`${propertyName}-input`"
      type="number"
      v-model="sliderValue"
      :step=step
      :min="min"
      :max="max"/>
    <label class="units">{{ units }}</label>
  </div>
</template>

<style src="@vueform/slider/themes/default.css"></style>
<style scoped>
  .single-tuner {
    margin: 1em;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
  }

  .single-tuner label {
    margin-bottom: 0.25rem;
    font-weight: 600;
  }

  .Slider {
    min-width: 150px;
  }

  input {
    max-width: 80px;
    margin: 10px;
  }

  label {
    margin: 0px 0px 0px 10px;
    min-width: 111px;
  }

  .units {
    margin: 0px 5px;
    min-width: 50px;
  }
</style>
