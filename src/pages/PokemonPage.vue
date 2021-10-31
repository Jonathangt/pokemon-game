<template>
    <div v-if="!pokemon">
        Espera
    </div>

    <div v-else>
      <h1>¿Quién es este pokemón?</h1>

      <pokemonPicture :pokemon-id="pokemon.id" :show-pokemon="showPokemon"/>
      <pokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAnswer"/>

      <templete v-if="showAnswer">
          <h2 class="fade-in">{{message}}</h2>
        <button @click="newGame">New game</button>
      </templete>
    </div>
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue';
import PokemonOptions from '@/components/PokemonOptions.vue';
import getPokemonsOptions from '@/helpers/getPokemonOptions'

export default {
  name: "PokemonPage",
  components: { 
    PokemonOptions, 
    PokemonPicture 
  },
  data() {
    return {
      pokemonArr:[],
      pokemon:null,
      showPokemon:false,
      showAnswer:false,
      message:'',
    }
  },
  methods:{
    /**
     * @description
     */
    async mixPokemonArr(){
      this.pokemonArr = await getPokemonsOptions()
      //genera un entero del 1 al 4
      const rndInt = Math.floor( Math.random() * 4)
      this.pokemon = this.pokemonArr[rndInt]
    },
    /**
     * @description
     * @param {Int} pokemonID
     */
    checkAnswer(pokemonID){
      pokemonID ? this.showPokemon = true : ""
      this.showAnswer = true

      if(pokemonID === this.pokemon.id){
        this.message = `Correcto, ${this.pokemon.name}`
      }else{
        this.message = `Ops era, ${this.pokemon.name}`
      }
    },
    /**
     * @description
     */
    newGame(){
      this.pokemonArr = []
      this.showPokemon = false
      this.showAnswer = false
      this.pokemon = null

      this.mixPokemonArr()
    }
  },
  mounted(){
    this.mixPokemonArr()
  }
};
</script>
