<script>
import pokemon_details from './components/pokemon_details.vue'
import pokedex_cards from './components/pokedex_cards.vue'
import menu_bar from './components/menu_bar.vue'
import memory from './components/memory.vue'

export default{
  components: {
    pokemon_details,
    pokedex_cards,
    menu_bar,
    memory,
  },

  data() {
    return {
      currentPokemon: null,
      componentDisplayed: 'pokedex_cards',
      pokemonData: null,
      filterKey:'',
      filterType1:'',
      filterType2:'',
      filterGen:'',
      memoryGeneration:'',
      memoryLevel:'Facile',
      typeColors: {
        Normal: '#A8A77A',
        Feu: '#EE8130',
        Eau: '#6390F0',
        Électrik: '#F7D02C',
        Plante: '#7AC74C',
        Glace: '#96D9D6',
        Combat: '#C22E28',
        Poison: '#A33EA1',
        Sol: '#E2BF65',
        Vol: '#A98FF3',
        Psy: '#F95587',
        Insecte: '#A6B91A',
        Roche: '#B6A136',
        Spectre: '#735797',
        Dragon: '#6F35FC',
        Ténèbres: '#705746',
        Acier: '#B7B7CE',
        Fée: '#D685AD',
      },
    }
  },

  methods: {
    async fetchData() {
      this.pokemonData = null
      const res = await fetch(
        `https://tyradex.vercel.app/api/v1/pokemon`
      )
      this.pokemonData = await res.json()
    },

    toggleDisplay(pok, component){
      this.componentDisplayed = component
      this.currentPokemon = pok
      console.log(this.componentDisplayed)
    },

    nextPokemon(){
      this.currentPokemon = this.filteredData[this.filteredData.indexOf(this.currentPokemon)+1]
    },

    previousPokemon(){
      this.currentPokemon = this.filteredData[this.filteredData.indexOf(this.currentPokemon)-1]
    },

  },

  computed: {
    filteredData(){
      if (!this.pokemonData) return []

      let newData=this.pokemonData.slice(1)
      if (this.filterType1){
        newData = newData.filter((pokemon) => (pokemon.types[0].name === this.filterType1) || (pokemon.types[1]?.name === this.filterType1))
      }
      if (this.filterType2){
        newData = newData.filter((pokemon) => (pokemon.types[0].name === this.filterType2) || (pokemon.types[1]?.name === this.filterType2))
      }
      if (this.filterGen){
        newData = newData.filter((pokemon) => (pokemon.generation === parseInt(this.filterGen)))
      }
      if (this.filterKey){
        let lowcaseKey=this.filterKey.toLowerCase()
        newData = newData.filter((pokemon) => (pokemon.name.fr.toLowerCase().startsWith(lowcaseKey) || pokemon.pokedex_id.toString().startsWith(this.filterKey)))
      }
      return(newData)
    },

  },

  async created() {
    await this.fetchData()
  },

}
</script>

<template>

  <div class="menu">
  <menu_bar :componentDisplayed="componentDisplayed"
            :typeColors="typeColors"
            :filteredData="filteredData"
            @toggleDisplay="(pok, component)=>toggleDisplay(pok, component)"
            @updateKey="(searchQuery) => filterKey=searchQuery"
            @updateType1="(type1Query) => filterType1=type1Query"
            @updateType2="(type2Query) => filterType2=type2Query"
            @updateGen="(number) => filterGen=number"
            @updateLevel="(level) => memoryLevel=level"
            @updateMemGen="(number) => memoryGeneration=number"
            />
  </div>

  <div class="available-view" :style="{alignContent: componentDisplayed==='pokedex_cards' ? 'start' : 'center'}">
    <div v-if="componentDisplayed==='pokedex_cards'" class="pokedex">
      <p v-if="!pokemonData">Chargement...</p>
      <pokedex_cards v-else
                    :filteredData="filteredData"
                    @toggleDisplay="(pok, component)=>toggleDisplay(pok, component)"/>
    </div>

    
    <pokemon_details  v-if="componentDisplayed==='pokemon_details'"
                     :pokemon="currentPokemon"
                     :filteredData="filteredData"
                     :typeColors="typeColors"
                     @next="nextPokemon()"
                     @prev="previousPokemon()"/>


    <div :class="(componentDisplayed==='memory') ? 'memory' : 'hidden'">
    <memory v-if="pokemonData"
            :pokemonData="pokemonData.slice(1)"
            :memoryGeneration="memoryGeneration"
            :level="memoryLevel"/>
    </div>

  </div>
</template>


<style>

.available-view{
  position:relative;
  flex:1;
  height:100%;
  width:100%;
  justify-items: center;
  margin-left: 30vw;
  
  @media screen and (max-width: 800px) {
    margin-left: 0;    
  }
}

.pokedex{
  position:relative;
  width:80%;
  padding-top: 3%;
  display: grid;
  justify-content:center;
  grid-template-columns: repeat(auto-fill, minmax(50px, 160px));
  gap: 20px; 

  @media screen and (max-width: 800px) {
    grid-template-columns: repeat(auto-fit, minmax(90px, 1fr));
    width:90%;
    padding-top: 10%;
    gap: 13px; 
  }
}

.hidden{
  display:none;
}

.memory{
  display:block;
}

</style>