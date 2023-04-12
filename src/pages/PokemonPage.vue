<template>
  <h1 v-if="!pokemon">Loading...</h1>
  <div v-else class="container">
    <h1>Who's this Pokemon</h1>
    <PokemonPicture
      :pokemonId="pokemon.id"
      :showPokemon="showPokemon"
    />
    <PokemonOptions
      :pokemons="pokemonArr"
      @selection="checkAnswer"
    />
    <template v-if="message">
      <h2 :class="isCorrect ? 'correct fade-in' : 'incorrect fade-in'">
        {{ message }}
      </h2>
      <button @click="newGame">
        New Game
      </button>
    </template>
  </div>
</template>

<script>
// @ significa que arranca desde el source
import PokemonOptions from "@/components/PokemonOptions.vue";
import PokemonPicture from "@/components/PokemonPicture.vue";
import getPokemonOptions from "@/helpers/getPokemonsOptions";

export default {
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      message: null,
      isCorrect: null,
    };
  },
  methods: {
    async mixedPokemons() {
      this.pokemonArr = await getPokemonOptions();

      this.pokemon = this.pokemonArr[Math.floor(Math.random() * 4)];
    },
    checkAnswer(pokemon) {
      if (pokemon === this.pokemon.id) {
        this.showPokemon = true;
        this.message = `Correct, the answer is ${
          this.pokemon.name.charAt(0).toUpperCase() + this.pokemon.name.slice(1)
        }`;
        this.isCorrect = true;
      } else {
        this.message = "Sorry, that answer is wrong";
        this.isCorrect = false;
      }
    },
    newGame(){
      this.mixedPokemons();
      this.pokemon = null;
      this.showPokemon = false;
      this.message = null;
      this.isCorrect = null;
    }
  },
  components: {
    PokemonOptions,
    PokemonPicture,
  },
  mounted() {
    this.mixedPokemons();
  },
};
</script>
<style scoped>
.container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.correct {
  color: green;
}

.incorrect {
  color: red;
}
</style>
