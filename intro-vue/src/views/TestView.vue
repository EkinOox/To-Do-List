<template>
  <div class="test">
    <h1 v-if="awesome">Vue est extraordinaire !</h1>
    <h1 v-else>Oh non 😢</h1>
    <div v-show="showDiv">Div tag with v-show</div>
    <button @click="toggle()">Basculer</button>
    <hr />

    <p>Valeur : {{ counterStore.count }}</p>
    <p>Double : {{ counterStore.doubleCount }}</p>
    <button @click="counterStore.increment">Incrémenter</button>
  </div>
</template>

<script setup lang="ts">
import { useCounterStore } from '@/stores/counter'
import { ref } from 'vue'

const awesome = ref(true);
const showDiv = ref(true);

function toggle() {
  awesome.value = !awesome.value;
  showDiv.value = !showDiv.value;
}

const counterStore = useCounterStore()

</script>

<style>
@media (min-width: 1024px) {
  .test {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }

  hr {
    width: 50%;
    margin: 20px;
  }
}
</style>