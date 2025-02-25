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

  <div class="bloc">
    <span :class="{arrowDisabled: prevDisabled}" @click="$emit('prev')" class="material-symbols-outlined left" > arrow_circle_right </span>
  </div>

  <div style="display:flex">
      <input id="star" class="star" type="checkbox" title="favorite" checked> 

      <div class="bloc-photo">
        <img class="img2" v-bind:src=pokemon.sprites.regular />
        <div class="bloc">
          <p> <b>{{ pokemon.name.fr }}</b> </p>
          <p> # {{ pokemon.pokedex_id }}</p>
        </div>
        <hr style="width:50%;"/>
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

<div class="bloc">
  <span :class="{arrowDisabled: nextDisabled}" @click="$emit('next')" class="material-symbols-outlined" > arrow_circle_right </span>
</div>

</div>

</template>


<style>

.img2{
  height:50%;
  width:auto;
  max-width: 250px;
  aspect-ratio: 1/1;
}

.container{
  position:relative;
  height:60vh;
  width:50vw;
  display:flex;
  justify-content: space-around;
  align-items: center;
  gap:8%;
  border-radius: 5px;
  box-shadow: 0px 0px 5px 5px rgba(0.2, 0.2, 0.2, 0.2);
  padding: 5%;
  padding:5%;
}

p{
  padding-top: 0px;
  margin: 0px;
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
  margin-top:5%;
}

.title{
  color:rgb(174, 174, 174);
  font-weight: 600;
  font-size:1.2rem;
}

.bloc{
  height:100%;
  width:fit-content;
  height: fit-content;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.bloc-photo{
  height:100%;
  width:fit-content;
  display: flex;
  flex-direction: column;
  gap:10px;
  align-items: center;
  justify-content: center;
  font-family:sans-serif;
}

.bloc-stats{
  flex:1;
  height:100%;
  width:fit-content;
  display: flex;
  flex-direction: column;
  gap:30px;
  align-items:start;
  justify-content: center;
  font-family:sans-serif;
}

.material-symbols-outlined{
  color:rgb(159, 159, 159);
  cursor:pointer;
  font-size:3rem;
}

.material-symbols-outlined.left{
  rotate:180deg;
}

.arrowDisabled{
  pointer-events: none;
  opacity:0.5;
}
</style>