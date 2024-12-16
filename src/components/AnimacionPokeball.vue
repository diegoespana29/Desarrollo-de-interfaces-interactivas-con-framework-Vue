<template>
  <div class="content-pokeball">
    <img
      class="img-pokeball pokeball1"
      src="../assets/pokeball.png"
      alt="pokeball"
    />
    <img
      class="img-pokeball pokeball2"
      src="../assets/pokeball.png"
      alt="pokeball"
    />
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
/* Pokébolas decorativas */
.content-pokeball {
  width: 100%;
  display: flex;
  justify-content: space-evenly;
  position: absolute;
}

.img-pokeball {
  width: 50px;
  height: 50px;
  animation: float 3s infinite ease-in-out;
}

/* Animación de movimiento */
@keyframes float {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}
@media (max-width: 768px) {
    .content-pokeball {
        justify-content: space-between;
    }
}
</style>
