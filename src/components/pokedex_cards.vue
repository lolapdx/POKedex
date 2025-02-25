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
    <input :id="pokemon.pokedex_id" class="star" type="checkbox" title="favorite" checked> 
    <br/>
    <img class="img1":src=pokemon.sprites.regular />
    <p> {{ pokemon.name.fr }} </p>
    <p> #  {{ pokemon.pokedex_id }} </p>
  </div>

</template>


<style>
.card{
  position: relative;
  align-items: center;
  text-align: center;
  border-radius: 5px;
  padding-bottom:5%;
  box-shadow: 0px 0px 10px 2px rgba(0.2, 0.2, 0.2, 0.2);
}

p{
  padding: 5px;
  margin: 0px;
}

.img1{
  width:70%;
}

</style>