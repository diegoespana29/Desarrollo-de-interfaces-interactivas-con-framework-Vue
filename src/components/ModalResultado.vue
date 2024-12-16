<template>
  <div class="modal-overlay">
    <div class="modal">
      <h2 class="modal-title">¡Resultados de la ronda!</h2>
      <p class="modal-text">
        <span class="highlight">Pokémon adivinados:</span> {{ pokemonadivinados.length }}
      </p>
      <p class="modal-text">
        <span class="highlight">Pokémon omitidos:</span> {{ pokemonOmitidos.length }}
      </p>
      <button ref="closeButton" class="btn-close" @click="cerrarModal">Cerrar</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pokemonadivinados: Array,
    pokemonOmitidos: Array,
  },
  methods: {
    cerrarModal() {
      const overlay = this.$el;
      overlay.classList.remove("show");
      setTimeout(() => {
        this.$emit("cerrarModal");
      }, 300);
    },
  },
  mounted() {
    this.$nextTick(() => {
      const overlay = this.$el;
      overlay.classList.add("show");
      setTimeout(() => {
        this.$refs.closeButton.focus();
      }, 300);
    });
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out;
  z-index: 1000;
}

.modal-overlay.show {
  opacity: 1;
  visibility: visible;
}

.modal {
  background: linear-gradient(
    180deg,
    #87cefa, /* Azul cielo claro */
    #ffeb99
  ); /* Degradado cielo */
  color: #ffffff;
  border-radius: 15px;
  padding: 20px;
  max-width: 90%;
  width: 400px;
  text-align: center;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5);
  transform: scale(0.8);
  transition: transform 0.3s ease-in-out;
}

.modal-overlay.show .modal {
  transform: scale(1);
}

.modal-title {
  font-size: 1.8rem;
  font-weight: bold;
  margin-bottom: 15px;
}

.modal-text {
  font-size: 1.2rem;
  margin: 10px 0;
}

.highlight {
  color: white;
  font-weight: bold;
}

.btn-close {
  margin: 20px auto;
  padding: 10px 20px;
  font-size: 18px;
  cursor: pointer;
  background: linear-gradient(135deg, #ffcb05, #3b4cca);
  border: none;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  color: white;
  transition: background 0.3s, transform 0.2s;
}

.btn-close:hover {
  background-color: #388e3c; /* Verde oscuro */
  transform: translateY(-2px);
}

.btn-close:active {
  transform: translateY(1px);
}
</style>
