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

   
    <div v-if="componentDisplayed==='pokedex_cards'" class="pokedex">
      <p v-if="!pokemonData">Chargement...</p>
      <pokedex_cards v-else
                    :filteredData="filteredData"
                    @toggleDisplay="(pok, component)=>toggleDisplay(pok, component)"/>
    </div>

    
    <div class="detailed-view" v-if="componentDisplayed==='pokemon_details'">
    <pokemon_details :pokemon="currentPokemon"
                     :filteredData="filteredData"
                     :typeColors="typeColors"
                     @next="nextPokemon()"
                     @prev="previousPokemon()"/>
    </div>

    <memory v-if="componentDisplayed==='memory'"
            :pokemonData="pokemonData"
            :memoryGeneration="memoryGeneration"
            :level="memoryLevel"/>

</template>


<style>

.detailed-view{
  position:absolute;
  height:90%;
  width:75%;
  align-content: center;
  justify-items: center;
  left:25%;
  margin-top: 2%;
}

.pokedex{
  position:absolute;
  left:25%;
  margin: 2% 8%;
  display: grid;
  grid-template-columns: repeat(4,1fr);
  row-gap: 30px;
  column-gap: 4%;
}

.star {
  position:relative;
  visibility:hidden;
  font-size:2.5rem;
  cursor:pointer;
  z-index:5;
  left:-30%;
}

.star:before {
 content: "\2605";
 position: relative;
 visibility:visible;
 color: rgb(255, 208, 0);
 z-index:5;

}
.star:checked:before {
 content: "\2606";
 position: relative;
 visibility: visible;
 z-index:5;
 color:darkgrey;
}
</style>