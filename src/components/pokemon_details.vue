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

    <button :class="{arrowDisabled: prevDisabled}" @click="$emit('prev')" class="navbutton">
      <span class="material-icons-outlined left" > arrow_circle_right </span>
    </button>

    <div class="inside-content">

      <div class="bloc-general">

        <img class="img2" v-bind:src=pokemon.sprites.regular />

        <div class="bloc">
          <p> <b>{{ pokemon.name.fr }}</b> </p>
          <p> # {{ pokemon.pokedex_id }}</p>
        </div>

        <hr style="width:50%;" v-if="!vertical"/>

        <div class="bloc">
          <div class="type-list" v-if="!vertical">
            <div class="type" :style="{backgroundColor: getColor(pokemon.types[0].name)}">{{ pokemon.types[0].name }}</div>
            <div class="type" :style="{backgroundColor: getColor(pokemon.types[1].name)}" v-if="pokemon.types[1]"> {{ pokemon.types[1].name }}</div>
          </div>

          <p class="category"> {{ pokemon.category }}</p>
        </div>
      </div>

      <div class="type-list" v-if="vertical">
              <div class="type" :style="{backgroundColor: getColor(pokemon.types[0].name)}">{{ pokemon.types[0].name }}</div>
              <div class="type" :style="{backgroundColor: getColor(pokemon.types[1].name)}" v-if="pokemon.types[1]"> {{ pokemon.types[1].name }}</div>
        </div>

      <div class="bloc-stats">
        <div v-if="!vertical">
          <p> <b>Taille :</b>{{ pokemon.height}}</p>
          <p> <b>Poids :</b> {{ pokemon.weight}}</p>
        </div>

        <div class="stats">

          <div class="title">
            <div>Statistiques de base</div>
            <hr style="width:100%;">
          </div>

          <div>
            <p> <b>HP :</b> {{ pokemon.stats.hp}}</p>
            <p> <b>Attaque :</b> {{ pokemon.stats.atk}}</p>
            <p> <b>Défense :</b> {{ pokemon.stats.def}}</p>
            <p> <b>Attaque spéciale :</b> {{ pokemon.stats.spe_atk}}</p>
            <p> <b>Défense spéciale :</b> {{ pokemon.stats.spe_def}}</p>
            <p> <b>Vitesse :</b> {{ pokemon.stats.vit}}</p>
            <br>

            <div class="physical" v-if="vertical">
              <p> <b>Taille :</b>{{ pokemon.height}}</p>
              <p> <b>Poids :</b> {{ pokemon.weight}}</p>
            </div>
          </div>

        </div>
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
  aspect-ratio: 1/1;

  @media screen and (max-width: 800px){
      max-width:20vh;
      min-width: 130px;;
    }
}

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

.bloc-general{
  flex:1;
  display:flex;
  flex-direction: column;
  gap:1vh;
  align-items: center;
  justify-content: center;
  font-family:sans-serif;
}

.bloc-stats{
  flex:1;
  width:30%;
  display: flex;
  flex-direction: column;
  gap:10%;
  align-items:start;
  justify-content: center;
  font-family:sans-serif;

  @media screen and (max-width: 800px){
      width:100%;
      gap:5vh;
    }
}

.bloc{
  height:fit-content;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.type-list{
  display: flex;
  gap:10%;
  justify-content: center;
}

.type{
  padding: 7px 14px 7px 14px;
  border-radius: 15px;
  text-align: center;

  @media screen and (max-width: 800px){
      width:50%;
    }
}

.category{
  color:rgb(174, 174, 174);
  font-weight: 600;
  letter-spacing: 1px;
  margin-top:4%;
  text-align: center;

  @media screen and (max-width: 800px){
      margin-top:0;
    }
}

.title{
  color:rgb(174, 174, 174);
  font-weight: 600;
  font-size:1.2rem;

  @media screen and (max-width: 800px){
      font-size:1.1rem;
      width:100%;
    }
}

.stats{
  display:flex;
  flex-direction: column;
  gap:2vh;

  @media screen and (max-width: 800px){
      gap:0.5vh;
    }
}

p{
  padding-top: 0px;
  margin: 0px;

  @media screen and (max-width: 800px){
      font-size:0.9rem;
    }
}

.bloc p {
  display: inline-block;
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