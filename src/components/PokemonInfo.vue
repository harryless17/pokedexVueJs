<template>
    <div id="card-info">

        <div v-if="show">
            <h1>Oups! Le pokemon n'existe pas</h1>
            <b-button @click="close" variant="outline-info">Retourner à la page precedente</b-button>
        </div>

        <b-card v-if="!show" id="b-card" :img-src="pokemon.sprites.front_default" img-alt="Image" img-top
            tag="article" class="mb-2 bg-dark text-white" >

            <b-card-text class="text-white rounded-0">
                <h1 v-if="pokemon">{{pokemon.species.name | capitalize}}
                    <b-badge pill variant="warning">{{pokemon.id}}</b-badge>
                </h1>
            </b-card-text>

            <span>Type(s): </span>

            <b-card-text > 
                <h5>
                <b-badge pill variant="success" v-for="(type, index) in pokemon.types"
                 :key="'poke'+index" style="display: inline">
                    {{type.type.name}}
                </b-badge>
                </h5>
            </b-card-text>
            <b-button @click="close" variant="outline-info">Choose an other pokemon</b-button>

        </b-card>
    </div>
</template>

<script>

    export default {
        name : 'PokemonInfo',

        props : [
            'pokemonUrl'
        ],

        data() {
            return {
                pokemon : {},
                show  : false,
            } 
        },

        methods: {
        
        //fetchData qui fait appelle à l'API et récupére la reponse et renvoie les données
        fetchData() {
            let req = new Request(this.pokemonUrl);
               console.log(req);
            fetch(req)
            .then((resp) => {
                if(resp.status === 200){
                    return resp.json();
                }else{
                    this.show = true;
                }
            })
            .then((data) => {
                this.pokemon = data;      
            })
            .catch((error) => {
                console.log(error);
            })
            
            
        },
        
        close() {
            this.$emit('closeDetail');
        }
        
      
    },
     
   filters: {
            capitalize: function (value) {
                if (!value) return ''
                value = value.toString()
                return value.charAt(0).toUpperCase() + value.slice(1)
            }
        },

    //Appel de fetchData() lorsque le composant est crée
    created() {
      this.fetchData();
    },

    
        
    }

</script>

<style>
    #card-info{
    margin-left: auto;
    margin-right: auto;
    width: 50%;
    height: 500px;
  }
    #b-card{
    margin-left: auto;
    margin-right: auto;
    width: 70%;
    height: 500px;
  }
</style>