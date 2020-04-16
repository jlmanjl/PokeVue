<template>
  <div class="pokecard row px-3 pb-5">
    <div class="col-sm-12 col-md-6 col-lg-6 my-1 px-2" v-for="pokemon in pokemons.slice(0, 151)" :key="pokemon.name">
      <div class="card my-2 mx-1 text-center nes-container is-rounded p-1 nes-pointer" :class="{selected:pokemon.id == selected}" @click="setPokemonURL(pokemon.url); selected = pokemon.id">
        <img class="mx-auto p-2 mt-4" :src="imageBaseURL + pokemon.id + '.png'" width="96px" height="96px">
        <div class="card-body">
          <p>{{ capitalize(pokemon.name) }}</p> 
          <P>#{{ pokemon.id }}</P>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeCard',
  props: [
    'pokemons',
    'imageBaseURL',
  ],  
  data() {
    return {
      selected: undefined
    }
  },
  methods: {
    capitalize(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    setPokemonURL(url) {
      this.$emit('setPokemonURL', url)
      console.log('pokemonURL has been emitted')
    }, 
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pokecard {
  max-height: 475px;
  overflow-y: scroll;
  overflow-x: hidden;
  color: #000;
}

.card {
  box-shadow: 0 4px 8px 0 rgb(41, 41, 41), 0 6px 20px 0 rgb(12, 12, 12);
  font-size: 14px;
  -webkit-transition: all 250ms cubic-bezier(.02, .01, .47, 1);
  -moz-transition: all 250ms cubic-bezier(.02, .01, .47, 1);
  transition: all 250ms cubic-bezier(.02, .01, .47, 1);
}

.card:hover {
	box-shadow: 0px 32px 60px rgba(0, 0, 0, 0.623);
	transform: translate(0px, -4px) !important;
	transition-delay: 0s !important;
	-webkit-transition: all 250ms cubic-bezier(.02, .01, .47, 1);
	-moz-transition: all 250ms cubic-bezier(.02, .01, .47, 1);
	transition: all 250ms cubic-bezier(.02, .01, .47, 1);
}

.selected {
  background-color: rgb(255, 67, 67);
  color: white;
  transition: 0.4s;
}

.selected img {
  border-radius: 50%;
  background-color: white;
}

::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  border-radius: 8px;
}

::-webkit-scrollbar-thumb {
  border-radius: 8px;
  background-color: rgb(255, 255, 255);
}
</style>
