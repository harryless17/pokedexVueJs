<template>
    <div  >
        <b-form id="searchBar" @submit="setPokemonUrl">
            <b-form-group
                id="input-group-1"
                label-for="input-1"
            >
                <b-form-input
                    id="input-1"     
                    type="text"
                    v-model="pokemonName"
                    placeholder="Ecrivez le nom complet du Pokemon">
                </b-form-input>
            </b-form-group>
        </b-form>
        
    </div>
</template>

<script>

export default {
    name : 'SearchPokemon',
    props : [
        'pokemonUrl',
    ],
    data () {
        return {
        pokemonName: '',
      }
    },
    methods: {
      setPokemonUrl(url) {
        if(this.searchvalue !== '')
          this.$emit('setPokemonUrl', this.pokemonUrl + this.pokemonName.toLowerCase());
      }
    }
  
}

</script>

<style>
 #searchBar{
    margin-left: auto;
    margin-right: auto;
    width: 50%;
 }
  
</style>