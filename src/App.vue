<script setup>
import ObjectTuner from './components/ObjectTuner.vue'
import SingleTuner from './components/SingleTuner.vue'
</script>

<template>
  <main>
    <h1>ObjectTuner Component for Vue.js</h1>
    <ObjectTuner boxTitle="Box title"/>
  </main>
</template>


<style scoped>
main {
  display: flex;
  flex-direction: column;
}

footer {
  font-style: italic;
}

@media screen and (max-width: 950px) {
  main {
    display: flex;
    flex-direction: column;
  }
}
</style>