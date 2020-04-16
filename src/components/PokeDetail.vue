<template>
  <div class="pokemon-detail px-5 nes-container is-rounded">
    <div class="detail-wrapper" v-if="!!selectedPokemon == true">
      <div class="pokemon-image text-center">
        <img :src="imageBaseURL + selectedPokemon.id + '.png'" width="200px" height="200px">
      </div>
      <div class="pokemon-info">
        <h3 class="name pt-2">#{{ selectedPokemon.id }} {{ selectedPokemon.name }}</h3>
        <div class="type" v-for="(value,index) in selectedPokemon.types" :key="index" >
          {{ value.type.name }}
        </div>

        <div class="description mt-5" v-if="description.flavor_text_entries != null">
          <div class="description-text">{{ description.flavor_text_entries.filter(function(element) {
            return element.language.name === 'en'})[0].flavor_text }}</div>
        </div>

        <div class="characteristics mt-5">
          <table>
            <tbody>
              <tr>
                <td>Height: {{ selectedPokemon.height*10 }}cm</td>
                <td>Weight: {{ selectedPokemon.weight/10 }}kg</td>
              </tr>
            </tbody>
          </table>
        </div>

        <div class="abilities mt-5">
          <div v-for="(object, index) in selectedPokemon.abilities" :key="index" class="ability">
            Ability {{ index + 1 }}: {{ object.ability.name }}
          </div>
        </div>  

        <div class="stats mt-5 nes-table-responsive">
          <h5>Stats</h5>
          <table class="stats-table nes-table is-bordered is-centered">
            <tbody>
              <tr>
                <td>HP</td>
                <td>{{ selectedPokemon.stats[5].base_stat }}</td>
              </tr>
              <tr>
                <td>Attack</td>
                <td>{{ selectedPokemon.stats[4].base_stat }}</td>
              </tr>
              <tr>
                <td>Special Attach</td>
                <td>{{ selectedPokemon.stats[2].base_stat }}</td>
              </tr>
              <tr>
                <td>Defense</td>
                <td>{{ selectedPokemon.stats[3].base_stat }}</td>
              </tr>
              <tr>
                <td>Special Defense</td>
                <td>{{ selectedPokemon.stats[1].base_stat }}</td>
              </tr>
              <tr>
                <td>Speed</td>
                <td>{{ selectedPokemon.stats[0].base_stat }}</td>
              </tr>
            </tbody>
          </table>
        </div>

        <div class="moves nes-table-responsive mt-5">
          <h5>Move List</h5>
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
  ],
  data() {
    return {
      description: {
        flavor_text_entries: null
      },
      selectedPokemonLoaded: false
    }
  },
  methods: {
    capitalize(string) {
        console.log(`run capitalization for ${this.selectedPokemon.name}`);
        return string.charAt(0).toUpperCase() + string.slice(1);
    },
    fetchDescription() {
      fetch(this.selectedPokemon.species.url)
      .then(res => {
        return res.json()
      })
      .then(this.setDescription)
      console.log('description fetched')       
    },
    setDescription(object) {
      this.description = object
    },
    loaded() {
      console.log('Mounted')
      return true
    }
  },
  watch: {   
    selectedPokemon: function(){
      this.fetchDescription() 
    }
  },
  updated() {
    this.loaded()
  }

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
  box-shadow: 0 4px 8px 0 rgb(41, 41, 41), 0 6px 20px 0 rgb(12, 12, 12);
}

.no-selection {
  height: 100%;
}

h3 {
  text-transform: capitalize;
  font-size: 18px;
}
h5 {
  font-size: 16px;
}
.ability {
  text-transform: capitalize;
}
.type {
  text-transform: capitalize;
}

.moves-table {
  width: 98%;
}

table {
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