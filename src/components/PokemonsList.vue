<template>
  <div id="list">
    <b-card id="card" title="Pokemon's List" class="mb-2 border-dark ">
      <b-list-group id="list_group">
        <b-list-group-item id_="list_group_item" v-for="(pokemon, index) in pokemons" :key="'poke'+index"
          class="mb-2 bg-dark text-white" @click="setPokemonUrl(pokemon.url)" button>

          {{ pokemon.name | capitalize }}
        </b-list-group-item>
        <div ref="infinitescrolltrigger"></div>

      </b-list-group>

    </b-card>

  </div>
</template>

<script>
  export default {
    name: 'PokemonsList',
    props: [
      'apiUrl',

    ],
    data: () => {
      return {
        pokemons: [],
        nextUrl: '',
        currentUrl: '',
      }
    },
    methods: {

      //fetchData qui fait appelle à l'API et récupére la reponse et renvoie les données
      fetchData() {
        let req = new Request(this.currentUrl);
        fetch(req)
          .then((resp) => {
            if (resp.status === 200)
              return resp.json();
          })
          .then((data) => {
            this.nextUrl = data.next;
            data.results.forEach(pokemon => {
              this.pokemons.push(pokemon);
            });
          })
          .catch((error) => {
            console.log(error);
          })
      },

      //Methode qui s'occupe du scroll avec pagination
      scrollTrigger() {
        const observer = new IntersectionObserver((entries) => {
          entries.forEach(entry => {
            if (entry.intersectionRatio > 0 && this.nextUrl) {
              this.next();
            }
          });
        });

        observer.observe(this.$refs.infinitescrolltrigger);
      },

      next() {
        this.currentUrl = this.nextUrl;
        this.fetchData();
      },

      //Recuperation de l'evenement du click sur le nom d'un pokemon
      setPokemonUrl(url) {
        this.$emit('setPokemonUrl', url);
      }
    },
    filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    },

    created() {
      this.currentUrl = this.apiUrl;
      this.fetchData();
    },
    mounted() {
      this.scrollTrigger()
    },
  }
</script>

<style>
  #list {
    margin-left: auto;
    margin-right: auto;
    width: 50%;
  }

  ;

  #card {
    margin-left: auto;
    margin-right: auto;
    width: 50%;
    height: 500px;

  }

  #list_group {
    margin-left: auto;
    margin-right: auto;
    width: auto;
    height: 400px;
    overflow: scroll;
    -webkit-overflow-scrolling: touch;
  }
</style>