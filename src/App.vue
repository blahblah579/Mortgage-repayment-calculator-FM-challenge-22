<template>
  <v-app>
    <v-main class="main d-flex align-center">
      <HomePage />
    </v-main>
  </v-app>
</template>

<script setup>
import HomePage from "./components/HomePage.vue";
</script>
<style>
.main {
  background-color: hsl(199, 81%, 94%);
}
</style>