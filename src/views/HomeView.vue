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
    const pokemonPromesas = data.results;
    obtenerPersonajes(pokemonPromesas);

    // console.log("resultados", pokemonPromesas);

    /* const pokemonPromesas = data.results.map(async (pokemon) => {
      const pokemonDetalles = await axios.get(pokemon.url);
      //console.log(pokemonDetalles);
      return {
        id: pokemonDetalles.data.id,
        name: pokemonDetalles.data.name,
        image: pokemonDetalles.data.sprites.front_default,
      };
    });*/

    // personajes.value = await Promise.all(pokemonPromesas);
    //console.log(personajes.value);
  } catch (error) {
    console.log(error);
  }
};

const obtenerPersonajes = async (data) => {
  for (let i = 0; i < data.length; i++) {
    let pokemon = data[i];
    let pokemonDetalles = await axios.get(pokemon.url);
    // console.log(pokemonDetalles.data.id);
    let pokemomInfo = {
      id: pokemonDetalles.data.id,
      name: pokemonDetalles.data.name,
      image: pokemonDetalles.data.sprites.front_default,
    };
    personajes.value.push(pokemomInfo);
  }
  console.log("personajes", personajes.value);
};
const paginacion = async (accion) => {
  if (accion == "next") {
    cont.value++;

    personajes.value = [];
    const { data } = await axios.get(next.value);
    obtenerPersonajes(data.results);
    next.value = data.next;
    prev.value = data.previous;
  }
  if (accion == "prev") {
    cont.value--;

    personajes.value = [];
    const { data } = await axios.get(prev.value);
    obtenerPersonajes(data.results);
    next.value = data.next;
    prev.value = data.previous;
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
