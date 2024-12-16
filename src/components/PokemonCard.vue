<template>
  <div class="card">
    <div class="card__content-img">
      <img class="img-quien" src="../assets/quien.png" alt="¿Quién es este Pokémon?" />
      <img
        ref="cardImage"
        :src="pokemon.sprites?.front_default"
        :alt="pokemon?.name"
        class="img-api"
        loading="lazy"
        :style="{ filter: mostrado ? 'none' : 'brightness(0)' }"
        :class="{ 'brillo-descubierto': adivinado }"
      />
      <div v-if="error" class="error-overlay">
        <img class="img-error" src="../assets/error.png" alt="Error" />
      </div>
    </div>
    <h3 v-if="mostrado" class="pokemon-nombre">{{ pokemon.name }}</h3>
    <div class="card-content" :style="{ display: mostrado ? 'none' : 'block' }">
      <input
        ref="pokemonInput"
        type="text"
        placeholder="Escriba el nombre"
        class="card-input"
        v-model="pokemonInput"
        @keyup.enter="descubrir"
        @keyup.esc="omitir"
        autocapitalize="none"
        autocomplete="off"
      />
      <div class="content-button">
        <button class="button-descubrir" @click="descubrir">Descubrir</button>
        <button class="button-omitir" @click="omitir">Omitir</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pokemonInput: "",
      mostrado: false,
      adivinado: false,
      error: false,
    };
  },
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },
  mounted() {
    this.$nextTick(() => {
      if (this.$refs.pokemonInput) {
        this.$refs.pokemonInput.focus();
      }
    });
  },
  methods: {
    descubrir() {
      if (this.pokemonInput.toLowerCase() === this.pokemon.name) {
        this.adivinado = true;
        this.mostrado = true;
        this.error = false;
        this.$nextTick(() => {
          this.$refs.cardImage.classList.add("brillo-descubierto");
          setTimeout(() => {
            this.$refs.cardImage.classList.remove("brillo-descubierto");
          }, 1500);
        });
        setTimeout(() => {
          this.$emit("pokemonAdivinado", this.adivinado);
          this.$emit("avanzarCard");
        }, 2500);
      } else {
        this.error = true;
        setTimeout(() => {
          this.error = false;
        }, 3000);
      }
    },
    omitir() {
      this.mostrado = true;
      this.adivinado = false;
      setTimeout(() => {
        this.$emit("pokemonOmitido", this.pokemon);
        this.$emit("avanzarCard");
      }, 2000);
    },
  },
  updated() {
    this.$nextTick(() => {
      if (this.$refs.pokemonInput) {
        this.$refs.pokemonInput.focus();
      }
    });
  },
};
</script>

<style scoped>
/* Estilos principales */
.card {
  padding: 15px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 90%;
  max-width: 400px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin: 10px auto;
  background-color: #ff0000;
  box-shadow: 0px -10px 15px -10px grey, 0px 10px 15px -10px grey;
}

/* Contenedor de imágenes */
.card__content-img {
  width: auto;
  height: auto;
  position: relative;
  margin: 0 auto;
}

.img-quien {
  width: 100%;
  height: auto;
  border-radius: 5px;
}

.img-api {
  width: 140px;
  height: auto;
  position: absolute;
  top: 50%;
  left: 30%;
  transform: translate(-50%, -50%);
}

.brillo-descubierto {
  animation: animacionBrillo 1.5s ease-in-out;
}

@keyframes animacionBrillo {
  0% {
    filter: brightness(1);
  }
  50% {
    filter: brightness(2.5);
  }
  100% {
    filter: brightness(1);
  }
}

/* Contenido y botones */
.card-content {
  padding: 10px;
}

.card-input {
  margin-top: 20px;
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 5px;
  height: 45px;
  font-size: 16px;
}

button {
  margin-top: 10px;
  width: 100%;
  height: 45px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  color: white;
  font-size: 14px;
}

.button-descubrir {
  background-color: #b3a125;
  transition: all 0.3s ease 0s;
}

.button-descubrir:hover {
  text-transform: uppercase;
  letter-spacing: 3px;
  box-shadow: 0px 5px 40px -10px #b3a125;
  background-color: #e1c82b;
  transition: all 0.3s ease 0s;
}

.button-omitir {
  background-color: #757575;
  transition: all 0.3s ease 0s;
}

.button-omitir:hover {
  background-color: #a5a5a5;
  text-transform: uppercase;
  letter-spacing: 3px;
  box-shadow: 0px 5px 40px -10px #a5a5a5;
  transition: all 0.3s ease 0s;
}

.content-button {
  display: flex;
  flex-direction: column;
  gap: 5px;
  padding: 0;
  margin-top: 20px;
}

/* Nombre del Pokémon */
.pokemon-nombre {
  color: white;
  font-size: 24px;
  font-weight: bold;
  text-align: center;
}

/* Error */
.error-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: aparecerDesaparecer 3s ease-in-out;
}

.img-error {
  width: 120px;
  height: 120px;
}

@keyframes aparecerDesaparecer {
  0% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.5);
  }
  10% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1.1);
  }
  90% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }
  100% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.5);
  }
}

/* Ajustes responsivos */
@media (min-width: 600px) {
  .card {
    width: 400px;
  }

  .img-api {
    width: 180px;
  }

  .card-input {
    font-size: 18px;
    height: 50px;
  }

  button {
    font-size: 16px;
    height: 50px;
  }

  .pokemon-nombre {
    font-size: 30px;
  }
}
</style>
