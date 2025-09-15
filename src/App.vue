<script setup>
import ObjectTuner from './components/ObjectTuner.vue'
</script>

<template>
  <main>
    <h1>ObjectTuner Component for Vue.js</h1>
    <ObjectTuner objectTitle="Object title"/>
  </main>
</template>


<style scoped>
main {
  display: flex;
  flex-direction: column;
}

h1 {
  text-align: center;
  font-size: 2rem;
  margin: 2rem 0;
  color: var(--color-heading);
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