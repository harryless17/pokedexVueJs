<template>
  <div id="app" class="container">
    <img src="../src/assets/pokedex.png" alt="">
    <SearchPokemon v-if="!showInfo" :pokemonUrl='apiUrl' @setPokemonUrl="setPokemonUrl"></SearchPokemon>
    <PokemonsList v-if="!showInfo" class=""  :apiUrl='apiUrl' @setPokemonUrl="setPokemonUrl"></PokemonsList>
    <PokemonInfo v-if="showInfo"  @closeDetail="closeDetail" ref="PokemonInfo" class="" :pokemonUrl="pokemonUrl"></PokemonInfo>
  </div>
</template>

<script>
import PokemonInfo from './components/PokemonInfo.vue';
import SearchPokemon from './components/SearchPokemon.vue'
import PokemonsList from './components/PokemonsList.vue';
export default {
  name: 'app',
  data: () => {
      return {
        //URL de Récuperation de la listes des pokemons        
        apiUrl : 'https://pokeapi.co/api/v2/pokemon/',

        //URL de Récuperation du pokemon
        pokemonUrl : 'https://pokeapi.co/api/v2/pokemon/1',

        //Bool pour gérer l'affichage des composants
        showInfo : false,
        
      }
    },
  components: {
    //Composants contenant les infos relatives au pokemon
    PokemonInfo,

    //Composant contenant la liste des pokemons
    PokemonsList,

    //Composant de la barre de recherche
    SearchPokemon
  },
  methods:{
    
    //Affiliation de l'URL du pokemon à afficher et affichage ou non du composant PokemonInfo
    setPokemonUrl(url){
      this.pokemonUrl = url;
      if (this.showInfo) {
      this.showInfo = false;
      }
      else
      this.showInfo = true;
    },

    //Fermeture du composant PokemonInfo
    closeDetail(){
      this.pokemonUrl = '';
      this.showInfo = false;
    }
  }
}

</script>

<style>
#app {
  text-align: center;
  margin-bottom: 40px;
}
</style>
