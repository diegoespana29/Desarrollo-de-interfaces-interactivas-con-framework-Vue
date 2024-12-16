<template>
  <div class="background-decorations">
    <!-- Estrellas -->
    <div class="star" style="top: 10%; left: 20%"></div>
    <div class="star" style="top: 30%; left: 70%"></div>
    <div class="star" style="top: 50%; left: 40%"></div>
    <div class="star" style="top: 70%; left: 30%"></div>
    <div class="star" style="top: 90%; left: 80%"></div>
    <div class="star" style="top: 100%; left: 50%"></div>
  </div>
</template>

<script>
export default {
  name: "Stars",
  data() {
    return {};
  },
  props: {
    star: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.star {
  position: absolute;
  width: 8px; /* Ajusta el tamaño */
  height: 8px;
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 50%;
  animation: twinkle 3s infinite alternate;
}

@keyframes twinkle {
  0%,
  100% {
    opacity: 0.5;
  }
  50% {
    opacity: 1;
  }
}
</style>
