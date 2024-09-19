<template>
  <nav class="navbar navbar-expand-sm navbar-ligth bg-warning">
    <div class="container">
      <a class="navbar-brand" href="#">
        <img src="/poke.png" alt="" width="60px" height="auto" />
        <span class="fs-3">Pokedex</span>
      </a>
      <button
        class="navbar-toggler d-lg-none"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#collapsibleNavId"
        aria-controls="collapsibleNavId"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="collapsibleNavId">
        <ul class="navbar-nav me-auto mt-2 mt-lg-0">
          <li class="nav-item">
            <RouterLink to="/" class="nav-link">Home </RouterLink>
          </li>
          <li class="nav-item">
            <RouterLink to="/about" class="nav-link">About</RouterLink>
          </li>
        </ul>
        <form class="d-flex my-2 my-lg-0">
          <input
            class="form-control me-sm-2"
            type="text"
            placeholder="Buscar"
          />
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">
            Buscar
          </button>
        </form>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { RouterLink } from "vue-router";
</script>

<style  scoped>
</style>