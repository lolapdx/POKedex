<script>
export default {

  props: {
    pokemon: Object,
    typeColors:Object,
    filteredData:Array,
  },

  emits: ['next', 'prev'],

  methods:{
    getColor(type){
      let color=this.typeColors[type]
      return color
    }
  },

  computed: {
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

    <button :class="{arrowDisabled: prevDisabled}" @click="$emit('prev')" class="navbutton">
      <span class="material-icons-outlined left" > arrow_circle_right </span>
    </button>

  <div class="bloc-photo">
    <img class="img2" v-bind:src=pokemon.sprites.regular />
    <div class="bloc">
      <p> <b>{{ pokemon.name.fr }}</b> </p>
      <p> # {{ pokemon.pokedex_id }}</p>
    </div>
    <hr style="width:50%;"/>
    <div class="bloc">
      <div class="type-list"> <div class="type" :style="{backgroundColor: getColor(pokemon.types[0].name)}">{{ pokemon.types[0].name }}</div> <div class="type" :style="{backgroundColor: getColor(pokemon.types[1].name)}" v-if="pokemon.types[1]"> {{ pokemon.types[1].name }}</div></div>
      <!-- <p> Generation  {{ pokemon.generation }}</p> -->
      <p class="category"> {{ pokemon.category }}</p>
    </div>
</div>

<div class="bloc-stats">
  <div>
    <p> <b>Taille :</b>{{ pokemon.height}}</p>
    <p> <b>Poids :</b> {{ pokemon.weight}}</p>
  </div>
  <div>
    <p class="title">Statistiques de base</p>
    <hr style="width:120%;">
  </div>
  <div>
    <p> <b>HP :</b> {{ pokemon.stats.hp}}</p>
    <p> <b>Attaque :</b> {{ pokemon.stats.atk}}</p>
    <p> <b>Défense :</b> {{ pokemon.stats.def}}</p>
    <p> <b>Attaque spéciale :</b> {{ pokemon.stats.spe_atk}}</p>
    <p> <b>Défense spéciale :</b> {{ pokemon.stats.spe_def}}</p>
    <p> <b>Vitesse :</b> {{ pokemon.stats.vit}}</p>
  </div>
</div>

  <button :class="{arrowDisabled: nextDisabled}" @click="$emit('next')" class="navbutton">
    <span class="material-icons-outlined" > arrow_circle_right </span>
  </button>

</div>

</template>


<style>

.img2{
  max-width:100%;
  height:auto;
  /* max-width: 250px; */
  aspect-ratio: 1/1;
}

.container{
  position:relative;
  height:60vh;
  width:60vw;
  display:flex;
  justify-content: space-around;
  align-items: center;
  gap:8%;
  border-radius: 5px;
  box-shadow: 0px 0px 5px 5px rgba(0.2, 0.2, 0.2, 0.2);
  padding: 2%;
}

p{
  padding-top: 0px;
  margin: 0px;
}

.bloc p {
  display: inline-block; /* Change to inline-block */
}

.type-list{
  display: flex;
  gap:10%;
  justify-content: center;
}

.type{
  padding: 7px 14px 7px 14px;
  border-radius: 15px;
}

.category{
  color:rgb(174, 174, 174);
  font-weight: 600;
  letter-spacing: 1px;
  margin-top:10%;
  text-align: center;
}

.title{
  color:rgb(174, 174, 174);
  font-weight: 600;
  font-size:1.2rem;
}

.bloc{
  height:100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.bloc-photo{
  height:fit-content;
  width:25%;
  display:flex;
  flex-direction: column;
  flex:1;
  gap:10px;
  align-items: center;
  font-family:sans-serif;
}

.bloc-stats{
  flex:1;
  height:100%;
  width:30%;
  display: flex;
  flex:1;
  flex-direction: column;
  gap:5%;
  align-items:start;
  justify-content: center;
  font-family:sans-serif;
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
}

.material-icons-outlined.left{
  transform : rotate(180deg);
}

.arrowDisabled{
  pointer-events: none;
  opacity:0.5;
}
</style>