<template>
  <div class="pikachu">
    <img src="../assets/pikachuRunning.gif" alt="Pikachu corriendo" />
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.pikachu {
  position: absolute;
  bottom: 490px;
  right: 100px;
  opacity: 0;
  z-index: 11;
  animation: pikachu-run 5s linear infinite;
  animation-delay: 5s;
}

@keyframes pikachu-run {
  0% {
    opacity: 1;
    left: -2000px;
  }

  100% {
    left: 110%;
    transform: translateX(100%);
  }
}
@media (max-width: 768px) {
  @keyframes pikachu-run {
    0% {
      opacity: 1;
      left: 0px;
    }

    100% {
      left: 80%;
      transform: translateX(10%);
    }
  }
  .pikachu {
    position: absolute;
    bottom: 400px;
  }
  img {
    width: 70px;
    height: 70px;
  }
}
</style>
