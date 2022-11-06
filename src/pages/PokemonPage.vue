<template>
    <span v-if="!pokemon">Cargando...</span>
  <div v-else>
    <h1>Quién es este pokémon?</h1>
     <PokemonImage :pokemon-id="pokemon.id" :show-pokemon="showPokemon"/>
     <PokemonOptions :pokemons="pokemons" @selection="checkAnswer"/>
    <div v-if="showAnswer">
        <h2>{{message}}</h2>
        <button @click="newGame">Jugar de nuevo</button>
    </div>
  </div>
</template>

<script>
import PokemonImage from '@/components/PokemonImage.vue';
import PokemonOptions from '@/components/PokemonOptions.vue';
import getPokemonOptions from '@/helpers/getPokemonOptions'


export default {
    components: { PokemonImage, PokemonOptions },
    data(){
        return{
            pokemons: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ""
        }
    },
    methods: {
        async mixPokemonArray(){
            this.pokemons = await getPokemonOptions()
            const randomInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemons[randomInt]
        },
        checkAnswer(id){
            if(this.pokemon.id === id){
                this.message = `Correcto! Es ${this.pokemon.name}`
            } else{

                this.message = `Ups! La opcion correcta era ${this.pokemon.name}`
            }
            this.showAnswer = true
            this.showPokemon = true
        },
        newGame(){
            this.showPokemon = false
            this.showAnswer = false
            this.message = ""
            this.pokemons = []
            this.pokemon = null
            this.mixPokemonArray()
        }
    },
    mounted(){
        this.mixPokemonArray()
    }
}
</script>