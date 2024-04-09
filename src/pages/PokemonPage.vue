<template>
  <h1 v-if="!pokemon">Espere porfavor...</h1>
  <div v-else>
    <h1>¿Quien es este pokemón?</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOption
      :pokemons="pokemonArr"
      @selection="checkAnswer"
      :showPokemon="showPokemon"
    />
    <h2>Tu puntaje es: {{ contador }}</h2>
    <template v-if="showAnswer">
      <h2>{{ message }}</h2>
      <button @click="newGame">Nuevo Juego</button>
    </template>
  </div>
</template>

<script>
import PokemonOption from "@/components/PokemonOption.vue";
import PokemonPicture from "@/components/PokemonPicture.vue";
import getPokemonOption from "../helpers/getPokemonOptions";

export default {
  components: {
    PokemonPicture,
    PokemonOption,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: "",
      contador: 0,
    };
  },
  methods: {
    async mixPokemonArr() {
      this.pokemonArr = await getPokemonOption();

      const rndInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonArr[rndInt];
    },
    checkAnswer(selectedId) {
      this.showPokemon = true;
      this.showAnswer = true;
      if (selectedId === this.pokemon.id) {
        this.contador++;
        this.message = `Correcto, ${this.pokemon.name}`;
      } else {
        this.message = `Oops, era ${this.pokemon.name}`;
      }
    },
    newGame() {
      this.pokemonArr = [];
      this.showPokemon = false;
      this.showAnswer = false;
      this.pokemon = null;
      this.mixPokemonArr();
    },
  },
  mounted() {
    this.mixPokemonArr();
  },
};
</script>
