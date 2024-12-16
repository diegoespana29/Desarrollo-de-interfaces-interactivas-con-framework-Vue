<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <div class="contenedor-img mb-4 col-12">
    <img
      class="img-titulo img-fluid"
      src="./assets/pokemonTitulo.png"
      alt="titulo-pokemon"
    />
  </div>
  <h2>¿Quién es ese Pokémon?</h2>

  <RouterView />

  

</template>

<style scoped>
/* Asegúrate de que el contenedor ocupe todo el ancho y alto de la pantalla */
.contenedor-img {
  display: flex;
  justify-content: center;
  width: 100%;
}

.img-titulo {
  width: 100%;
  max-width: 500px; /* Tamaño máximo de la imagen */
  height: auto;
}

h2 {
  font-size: 1.5em;
  color: white;
  text-align: center;
  font-size: 2.5rem; /* Tamaño de fuente más grande */
  margin-top: 20px;
  margin-bottom: 20px;
  font-family: cursive;
}

@media (max-width: 768px) {
  h2 {
    font-size: 1.8rem; /* Ajusta el tamaño del título en pantallas pequeñas */
  }
}






</style>
