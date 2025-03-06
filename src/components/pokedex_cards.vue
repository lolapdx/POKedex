<script>
export default {

  props: {
    filteredData: Array,
  },

  emits: ['toggleDisplay'],

}

</script>


<template>

  <p v-if="!filteredData">Chargement...</p>

  <div class="card" v-else v-for="pokemon in filteredData" :key="pokemon.pokedex_id" @click="$emit('toggleDisplay',pokemon,'pokemon_details')">
    <img class="img1":src=pokemon.sprites.regular />
    <p style="letter-spacing: 0.5px;"> {{ pokemon.name.fr }} </p>
    <p> #  {{ pokemon.pokedex_id }} </p>
  </div>

</template>


<style>
.card{
  align-content: center;
  text-align: center;
  border-radius: 5px;
  height: 250px;
  box-shadow: 0px 0px 10px 2px rgba(0.2, 0.2, 0.2, 0.2);
  width: 100%;
  overflow: hidden;
  flex-shrink: 0;

  @media screen and (max-width: 800px) {
    width: 100%;
    height: 180px;
  }
}

p{
  padding: 4px;
  margin: 0px;
}

.img1{
  width:80%;
  height:auto;
}

</style>