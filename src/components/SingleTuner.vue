<script setup>
import { ref, watch } from 'vue'
import Slider from '@vueform/slider'

const props = defineProps({
  propertyName: String,
  value: Number,
  min: Number,
  max: Number,
  step: Number,
  units: String
})

const emit = defineEmits(['update:value'])
const sliderValue = ref(0)

watch(
  () => props.value,
  (newVal) => {
    if (newVal !== sliderValue.value) {
      sliderValue.value = newVal
    }
  },
  { immediate: true }
)

watch(sliderValue, (newVal) => {
  emit('update:value', newVal)
})
</script>

<template>
  <div class="single-tuner" style="margin: 1rem 0;">
    <label> {{ propertyName }} </label>
    <Slider class="Slider"
      v-model="sliderValue"
      :min="min"
      :max="max"
      :step="step"
      :lazy="false"
      :tooltips="false"
    />
    <input type="number" v-model="sliderValue" :step=step :min="min" :max="max"/>
    <label class="units">{{ units }}</label>
  </div>
</template>

<style src="@vueform/slider/themes/default.css"></style>
<style scoped>
  .single-tuner label {
    margin-bottom: 0.25rem;
    font-weight: 600;
  }
  .single-tuner {
    margin: em;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
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
