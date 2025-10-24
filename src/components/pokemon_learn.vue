<script>
export default {

  props: {
    pokemon: Object,
    typeColors: Object,
    filteredData: Array,
  },

  methods: {
    getColor(type) {
      let color = this.typeColors[type]
      return color
    }
  },

  computed: {
    vertical() {
      return (window.innerWidth < 800)
    },
  }
}
</script>


<template>
  <div class="bloc-general">

    <img class="img2" v-bind:src=pokemon.sprites.regular />

    <div class="bloc">
      <p> <b>{{ pokemon.name.fr }}</b> </p>
      <p> # {{ pokemon.pokedex_id }}</p>
    </div>

    <hr style="width:50%;" v-if="!vertical" />

    <div class="bloc">
      <div class="type-list" v-if="!vertical">
        <div class="type" :style="{ backgroundColor: getColor(pokemon.types[0].name) }">{{ pokemon.types[0].name }}
        </div>
        <div class="type" :style="{ backgroundColor: getColor(pokemon.types[1].name) }" v-if="pokemon.types[1]"> {{
          pokemon.types[1].name }}</div>
      </div>

      <p class="category"> {{ pokemon.category }}</p>
    </div>
  </div>

  <div class="type-list" v-if="vertical">
    <div class="type" :style="{ backgroundColor: getColor(pokemon.types[0].name) }">{{ pokemon.types[0].name }}</div>
    <div class="type" :style="{ backgroundColor: getColor(pokemon.types[1].name) }" v-if="pokemon.types[1]"> {{
      pokemon.types[1].name }}</div>
  </div>

  <div class="bloc-stats">
    <div v-if="!vertical">
      <p> <b>Taille :</b>{{ pokemon.height }}</p>
      <p> <b>Poids :</b> {{ pokemon.weight }}</p>
    </div>

    <div class="stats">

      <div class="title">
        <div>Statistiques de base</div>
        <hr style="width:100%;">
      </div>

      <div>
        <p> <b>HP :</b> {{ pokemon.stats.hp }}</p>
        <p> <b>Attaque :</b> {{ pokemon.stats.atk }}</p>
        <p> <b>Défense :</b> {{ pokemon.stats.def }}</p>
        <p> <b>Attaque spéciale :</b> {{ pokemon.stats.spe_atk }}</p>
        <p> <b>Défense spéciale :</b> {{ pokemon.stats.spe_def }}</p>
        <p> <b>Vitesse :</b> {{ pokemon.stats.vit }}</p>
        <br>

        <div class="physical" v-if="vertical">
          <p> <b>Taille :</b>{{ pokemon.height }}</p>
          <p> <b>Poids :</b> {{ pokemon.weight }}</p>
        </div>
      </div>

    </div>
  </div>

</template>

<style lang="css">

.img2{
  max-width:100%;
  height:auto;
  aspect-ratio: 1/1;

  @media screen and (max-width: 800px){
      max-width:20vh;
      min-width: 130px;;
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

</style>