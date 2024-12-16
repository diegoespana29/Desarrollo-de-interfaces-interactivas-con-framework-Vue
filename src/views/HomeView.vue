<template>
  <h2 v-if="juegoIniciado">
    Pokémon descubiertos: <span class="contador">{{ contador }}</span>
  </h2>
  <main>
    <div v-if="!juegoIniciado" class="text-center content-selection">
      <label for="generationSelect" class="form-label">Selecciona una Generación:</label>
      <select
        id="generationSelect"
        class="form-select mb-3"
        v-model="selectedGeneration"
      >
        <option value="all">Todos</option>
        <option value="gen1">Generación 1</option>
        <option value="gen2">Generación 2</option>
        <option value="gen3">Generación 3</option>
        <option value="gen4">Generación 4</option>
        <option value="gen5">Generación 5</option>
        <option value="gen6">Generación 6</option>
        <option value="gen7">Generación 7</option>
      </select>

      <label for="difficulty" class="form-label">Selecciona Dificultad:</label>
      <select id="difficulty" class="form-select mb-3" v-model="difficulty">
        <option value="easy">Fácil</option>
        <option value="medium">Medio</option>
        <option value="hard">Difícil</option>
      </select>

      <button @click="startGame" class="btn-start">Iniciar Juego</button>
    </div>

    <div v-if="juegoIniciado" class="card-container">
      <div v-for="(pokemon, index) in pokemons" :key="pokemon.id">
        <PokemonCard
          v-show="index === currentCard"
          :pokemon="pokemon"
          @guessed="handleGuess"
          @skipped="skipPokemon"
          @nextCard="nextCard"
        />
      </div>
    </div>

    <ModalResultado
      v-if="modalVisible"
      :guessedPokemons="guessedPokemons"
      :skippedPokemons="skippedPokemons"
      @close="resetGame"
    />

    <Stars />
    <AnimacionPikachu />
    <AnimacionPokeball />
  </main>
</template>


<script>
import PokemonCard from "../components/PokemonCard.vue";
import ModalResultado from "../components/ModalResultado.vue";
import Stars from "@/components/Stars.vue";
import AnimacionPikachu from "@/components/AnimacionPikachu.vue";
import AnimacionPokeball from "@/components/AnimacionPokeball.vue";
import axios from "axios";

export default {
  components: {
    PokemonCard,
    ModalResultado,
    Stars,
    AnimacionPikachu,
    AnimacionPokeball,
  },
  data() {
    return {
      pokemons: [],
      guessedPokemons: [],
      skippedPokemons: [],
      juegoIniciado: false,
      modalVisible: false,
      totalRounds: 0,
      currentCard: 0,
      selectedGeneration: "all",
      difficulty: "easy",
    };
  },
  methods: {
    async startGame() {
      this.resetGame();
      this.setRoundsByDifficulty();

      const { startId, endId } = this.getGenerationRange();

      const randomIds = new Set();
      while (randomIds.size < this.totalRounds) {
        randomIds.add(Math.floor(Math.random() * (endId - startId + 1)) + startId);
      }

      const idsArray = Array.from(randomIds);
      const pokemonData = await axios.all(
        idsArray.map((id) => axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`))
      );

      this.pokemons = pokemonData.map((response) => response.data);
      this.juegoIniciado = true;
    },
    setRoundsByDifficulty() {
      const difficultyLevels = { easy: 5, medium: 10, hard: 20 };
      this.totalRounds = difficultyLevels[this.difficulty] || 5;
    },
    getGenerationRange() {
      const ranges = {
        gen1: { startId: 1, endId: 151 },
        gen2: { startId: 152, endId: 251 },
        gen3: { startId: 252, endId: 386 },
        gen4: { startId: 387, endId: 493 },
        gen5: { startId: 494, endId: 649 },
        gen6: { startId: 650, endId: 721 },
        gen7: { startId: 722, endId: 809 },
        all: { startId: 1, endId: 809 },
      };
      return ranges[this.selectedGeneration] || ranges.all;
    },
    handleGuess(pokemon) {
      this.guessedPokemons.push(pokemon);
      this.checkIfGameEnds();
    },
    skipPokemon(pokemon) {
      this.skippedPokemons.push(pokemon);
      this.checkIfGameEnds();
    },
    checkIfGameEnds() {
      if (this.guessedPokemons.length + this.skippedPokemons.length === this.totalRounds) {
        this.modalVisible = true;
      }
    },
    nextCard() {
      if (this.currentCard < this.pokemons.length - 1) {
        this.currentCard++;
      }
    },
    resetGame() {
      this.juegoIniciado = false;
      this.pokemons = [];
      this.guessedPokemons = [];
      this.skippedPokemons = [];
      this.currentCard = 0;
      this.modalVisible = false;
    },
  },
  computed: {
    contador() {
      return this.guessedPokemons.length;
    },
  },
};
</script>


<style>
h2 {
  text-align: center;
  color: white;
}
main {
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}
label {
  font-size: 20px;
  font-weight: bold;
  color: white;
  margin-top: 40px;
}
.content-selection {
  display: flex;
  flex-direction: column;
  gap: 15px;
}
.btn-start {
  margin: 20px auto;
  padding: 12px 25px;
  font-size: 18px;
  cursor: pointer;
  background: linear-gradient(135deg, #ffcb05, #3b4cca);
  border: none;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  color: white;
  transition: background 0.3s, transform 0.2s;
}

.btn-start:hover {
  background: linear-gradient(135deg, #ffd700, #27408b);
  transform: translateY(-3px);
}

select {
  padding: 10px;
  font-size: 16px;
  border-radius: 8px;
  border: 2px solid #3b4cca;
  background-color: #f8f9fa;
  color: #3b4cca;
  transition: border-color 0.3s;
}

select:focus {
  border-color: #ffcb05;
  outline: none;
}

.card-container {
  display: flex;
  justify-content: center;
  height: 80vh;
  width: 100%;
  position: relative;
}

.pokemon-card {
  width: 200px;
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>

