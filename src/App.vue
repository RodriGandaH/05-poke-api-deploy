<script setup>
import { RouterView } from "vue-router";
import Footer from "./components/Footer.vue";
import Navbar from "./components/Navbar.vue";

const name = "Rodrigo Gandarillas Heredia";
</script>

<template>
  <div class="contenedor">
    <header>
      <Navbar />
    </header>
    <main>
      <RouterView />
    </main>
    <footer>
      <Footer :name="name" />
    </footer>
  </div>
</template>

<style scoped>
main,
header {
  flex: 1;
}
.contenedor {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: rgba(231, 180, 85, 0.815);
}
</style>
