<script setup>
import { RouterView } from "vue-router";
import NavBar from "./components/NavBar.vue";
import Footer from "./components/Footer.vue";
</script>

<template>
  <div class="app-layout">
    <NavBar />
    <main>
      <RouterView />
    </main>
    <Footer />
  </div>
</template>

<style scoped>
.app-layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

main {
  flex: 1;
}
</style>
