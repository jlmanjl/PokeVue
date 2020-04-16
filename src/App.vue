<template>
  <div id="app" class="container">
    <div class="header text-center">
        <img src="./assets/pokemonlogo.png" width="300px" alt="" class="m-3">
        <h1>Jimothy's Pokedex.</h1>
    </div>

    <div class="row my-3">
      <div class="col-sm-12 col-lg-6 mt-4" >
        <poke-detail 
          :imageBaseURL="imageBaseURL"
          :selectedPokemon="selectedPokemon"
        />
      </div>
      <div class="col-sm-12 col-lg-6">
        <div class="nes-container is-rounded is-dark mt-4 search-container ">
          <search-bar 
            @updateQuery="updateQuery($event)"
          />
          <poke-card class="searched"
            v-if="this.query === ''"
            :pokemons="pokemons"
            :imageBaseURL="imageBaseURL"
            @setPokemonURL='setPokemonURL($event)'
          />
          <poke-card class="no-search"
            v-else
            :pokemons="filteredPokemons"
            :imageBaseURL="imageBaseURL"
            @setPokemonURL='setPokemonURL($event)'
          />
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import PokeCard from './components/PokeCard.vue'
import SearchBar from './components/SearchBar.vue'
import PokeDetail from './components/PokeDetail.vue'

export default {
  name: 'App',
  components: {
    PokeCard,
    SearchBar,
    PokeDetail
  },
  data () {
    return {
      pokemons: [],
      imageBaseURL: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
      query: '',
      pokemonURL: 'https://pokeapi.co/api/v2/pokemon/1',
      selectedPokemon: '',
    }
  },
  methods: {
    getPokemonData () {
        const url = `https://pokeapi.co/api/v2/pokemon/?limit=900`;
        fetch(url)
        .then(res => {
          return res.json()})
        .then((data) => {
          data.results.forEach(pokemon => {
            pokemon.id = pokemon.url.split('/')
              .filter(function(part) { return !!part }).pop()
            this.pokemons.push(pokemon);
          })
        })
        console.log('Fetching Pokemon Data successful')
    },
    fetchSelectedPokemonData(url) {
      console.log('Fetching Selected Pokemon')
      fetch(url)
      .then(res => {
          return res.json();
      })
      .then(this.setSelected);
      console.log('Fetching Selected Pokemon Successful')
    },
    setSelected (results) {
      this.selectedPokemon = results;
      console.log(`${this.selectedPokemon.name} has been selected`)
    },
    updateQuery(query) {
      this.query = query;
    },
    setPokemonURL(url) {
      this.pokemonURL = url;
      console.log("PokemonURL has been set");
      this.fetchSelectedPokemonData(url);
    },
  },
  async created() {
    this.getPokemonData();

  },
  computed: {
    filteredPokemons() {
      return this.pokemons.filter(pokemon => {
        return pokemon.name.includes(this.query)
      })
    }
  }
}
</script>

<style>
body {
  background-image: url('./assets/pokemon bg.png');
  background-position: center;
  background-size: cover;
  }

.search-container {
  max-height: 600px;
  border-radius: 8px;
  background-color: rgb(39, 39, 39);
  outline: none;
  box-shadow: 0 4px 8px 0 rgb(41, 41, 41), 0 6px 20px 0 rgb(12, 12, 12);
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000000;
}

.header {
  color: white;
}

h1{
  font-size: 16px;
}
</style>
