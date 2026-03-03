<script>
import pokemon_details from './pokemon_details.vue'
import pokemon_learn from './pokemon_learn.vue'

export default {

components: {
    pokemon_details,
    pokemon_learn,
},

  props: {
    pokemon: Object,
    typeColors:Object,
    filteredData:Array,
    mode:String,
  },

  data(){
    return {
      learningStep:1,
    }
  },

  emits: ['next', 'prev'],

  methods:{
    getColor(type){
      let color=this.typeColors[type]
      return color
    }
  },

  computed: {
    vertical(){
      return (window.innerWidth < 800)
    },

    prevDisabled() {
      return (this.pokemon===this.filteredData[0] || !this.filteredData.includes(this.pokemon))
    },
    nextDisabled() {
      return (this.pokemon===this.filteredData[this.filteredData.length-1] || !this.filteredData.includes(this.pokemon))
    },
  }
}
</script>


<template>
  <p v-if="!pokemon">Chargement...</p>

  <div v-else class="container">

    <button :class="{arrowDisabled: prevDisabled}" @click="$emit('prev'), learningStep=1" class="navbutton">
      <span class="material-icons-outlined left" > arrow_circle_right </span>
    </button>

    <div class="inside-content">

      <pokemon_details v-if="mode==='details'"
                        :pokemon="pokemon"
                        :typeColors="typeColors"
                        :filteredData="filteredData"
                        />

      <pokemon_learn v-else-if="mode==='learn'"
                        :pokemon="pokemon"
                        :typeColors="typeColors"
                        :filteredData="filteredData"
                        :step="learningStep"
                        @nextStep="learningStep = (learningStep % 3) + 1"
                        />
                        
    </div>

  <button :class="{arrowDisabled: nextDisabled}" @click="$emit('next'), learningStep=1" class="navbutton">
    <span class="material-icons-outlined" > arrow_circle_right </span>
  </button>

</div>

</template>


<style>

.container{
  position:relative;
  height:60vh;
  min-height: 450px;
  width:60vw;
  display:flex;
  justify-content: space-around;
  align-items: center;
  gap:5%;
  border-radius: 5px;
  box-shadow: 0px 0px 5px 5px rgba(0.2, 0.2, 0.2, 0.2);
  padding: 2%;
  margin:6%;

  @media screen and (max-width: 800px){
      gap:4%;
      height:85%;
      min-height: 600px;
      width:80vw;
    }
}

.inside-content{
  display:flex;
  align-items: space-around;
  justify-content: center;
  flex:1;
  gap:10%;


  @media screen and (max-width: 800px){
      flex-direction: column;
      gap:3vh;
      width:60%;
    }
}

.navbutton{
  background-color:white;
  border:none;
  width:12%;
}
.material-icons-outlined{
  color:rgb(159, 159, 159);
  cursor:pointer;
  font-size:3rem;
  
  @media screen and (max-width: 800px){
      font-size:1.5rem;
  }
}

.material-icons-outlined.left{
  transform : rotate(180deg);
}

.arrowDisabled{
  pointer-events: none;
  opacity:0.5;
}
</style>