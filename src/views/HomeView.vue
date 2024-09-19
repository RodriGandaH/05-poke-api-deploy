<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";

const urlBase = "https://pokeapi.co/api/v2/pokemon?limit=18";
const personajes = ref([]);
const next = ref(null);
const prev = ref(null);
let cont = ref(1);

onMounted(() => {
  cargarPersonajes();
});
const cargarPersonajes = async () => {
  try {
    const { data } = await axios.get(urlBase);
    console.log(data);
    next.value = data.next;
    prev.value = data.previous;
    console.log("siguiente", next.value);

    const pokemonPromesas = await Promise.all(
      data.results.map(async (pokemon) => {
        const pokemonDetalles = await axios.get(pokemon.url);
        return {
          id: pokemonDetalles.data.id,
          name: pokemonDetalles.data.name,
          image: pokemonDetalles.data.sprites.front_default,
        };
      })
    );

    personajes.value = pokemonPromesas;
    console.log("personajes", personajes.value);
  } catch (error) {
    console.log(error);
  }
};

const paginacion = async (accion) => {
  try {
    let url = accion === "next" ? next.value : prev.value;
    if (!url) return;

    cont.value += accion === "next" ? 1 : -1;
    personajes.value = [];

    const { data } = await axios.get(url);
    next.value = data.next;
    prev.value = data.previous;

    const pokemonPromesas = await Promise.all(
      data.results.map(async (pokemon) => {
        const pokemonDetalles = await axios.get(pokemon.url);
        return {
          id: pokemonDetalles.data.id,
          name: pokemonDetalles.data.name,
          image: pokemonDetalles.data.sprites.front_default,
        };
      })
    );

    personajes.value = pokemonPromesas;
  } catch (error) {
    console.log(error);
  }
};
</script>

<template>
  <div class="container my-5">
    <div class="row justify-content-center g-3">
      <div
        v-for="pokemon in personajes"
        :key="pokemon.id"
        class="card bg-warning mx-2"
      >
        <h5 class="card-title text-center">{{ pokemon.name }}</h5>

        <div class="card-body p-0 text-center">
          <img :src="pokemon.image" class="card-img-top" alt="imagen" />
          <h4 class="card-text">{{ pokemon.id }}</h4>
        </div>
      </div>
    </div>
    <div class="row mt-2">
      <div class="col-md-4 text-center">
        <button
          v-if="prev != null"
          class="btn btn-dark"
          @click="paginacion('prev')"
        >
          <i class="fa fa-arrow-circle-o-left fs-5" aria-hidden="true"></i>
        </button>
      </div>
      <div class="col-md-4 text-center">
        <span>Página {{ cont }} </span>
      </div>
      <div class="col-md-4 text-center">
        <button
          v-if="next != null"
          class="btn btn-dark"
          @click="paginacion('next')"
        >
          <i class="fa fa-arrow-circle-o-right fs-5" aria-hidden="true"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 200px;
}
img {
  height: 100px;
  width: 100px;
}
</style>
