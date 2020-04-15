<template>
  <div class="pokemon-detail px-5 nes-container is-rounded">
    <div class="detail-wrapper" v-if="!!selectedPokemon == true">
      <div class="pokemon-image text-center">
        <img :src="imageBaseURL + selectedPokemon.id + '.png'" width="200px" height="200px">
      </div>
      <div class="pokemon-info">
        <h3 class="name pt-2">#{{ selectedPokemon.id }} {{ capitalize(selectedPokemon.name) }}</h3>
        <div class="type" v-for="(value,index) in selectedPokemon.types" :key="index" >
          {{ capitalize(value.type.name) }}
        </div>

        <div class="characteristics mt-4">
          <div>Height: {{ selectedPokemon.height*10 }}cm</div>
          <div>Weight: {{ selectedPokemon.weight/10 }}kg</div>
        </div>

        <div class="stats mt-4">
          Stats
          <div class="row">
            <div class="col-6" v-for="(object, index) in selectedPokemon.stats" :key="index">
              {{ capitalize(object.stat.name) }}: {{ object.base_stat }}
            </div>
          </div>
        </div>

        <div class="abilities mt-4">
          <div v-for="(object, index) in selectedPokemon.abilities" :key="index" >
            Ability {{ index + 1 }}: {{ capitalize(object.ability.name) }}
          </div>
        </div>
        <div class="moves nes-table-responsive mt-5">
          Move List
          <table class="moves-table nes-table is-bordered is-centered">
            <thead>
              <tr>
                <th>Move</th>
                <th>Learned By</th>
              </tr>
            </thead>
            <tbody class="text-center">
              <tr v-for="(object, index) in selectedPokemon.moves" :key="index">
                <td>{{ object.move.name }}</td>
                <td>{{ object.version_group_details[0].move_learn_method.name }}</td>
              </tr>
            </tbody>
          </table>
        </div>

      </div>
    </div>

    <div class="row h-100"  v-else>
      <div class="no-selection text-center col-sm-12 my-auto" >
        <div>
          <i class="nes-ash m-5"></i>
          <!-- Balloon -->
          <div class="nes-container">
            <p>Select a Pokemon to learn more about it!</p>
          </div>          
        </div>

      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'PokeDetail',
  props: [
    'imageBaseURL',
    'selectedPokemon',
    'description'
  ],
  methods: {
    capitalize(string) {
        console.log(`run capitalization for ${this.selectedPokemon.name}`);
        return string.charAt(0).toUpperCase() + string.slice(1);
    },
    sortByProperty(property) {
      return function(a, b) {
        if(a[property] > b[property])
          return 1;
        else if(a[property] < b[property])
          return -1;
        
        return 0;
      }
    },
    // extractPokeID(pokemonURL) {
    //   this.speciesURL = "https://pokeapi.co/api/v2/pokemon-species/" + pokemonURL.split('/')
    //   .filter(function(part) { return !!part }).pop();
    // },
  },
  // created() {   
  //   this.fetchSelectedPokemonData('https://pokeapi.co/api/v2/pokemon/1/')
  // }
}

</script>

<style>
.pokemon-detail {
  margin-bottom: 24px;
  padding: 24px;
  background-color: white;
  max-height: 600px;
  overflow-y: scroll;
  font-size: 12px;
}

.no-selection {
  height: 100%;
}

.moves-table {
  width: 98%;
}

::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  border-radius: 8px;
}

::-webkit-scrollbar-thumb {
  border-radius: 8px;
  background-color: rgb(255, 67, 67);
}

.detail-wrapper {
  transition: 0.4s;
}

</style>