<script>
  import { nextTick } from 'vue';

export default {
  data(){
    return{
      pokemonPairsList:[],
      firstTile:null,
      turnEnd:true,
      gameEnd:false,
      displayMoyenInput:false,
      displayDifficileInput:false,
      inputValue:'',
      resolveValidation: null,
      answer:'',
    }
  },

  props: {
    pokemonData: Object,
    memoryGeneration: String,
    level: String,
    isInitialized: Boolean,
  },

  methods: {
    choosePokemon(){
      //resets
      var pokemon=[];
      this.firstTile=null;
      this.turnEnd=true;
      this.gameEnd=false;
      this.displayMoyenInput=false;
      this.displayDifficileInput=false;
      this.inputValue='';
      this.resolveValidation= null;
      this.answer='';

      while (pokemon.length < 16){
        const randomIndex = Math.floor(Math.random() * this.pokemonList.length);
        const randomPokemon = this.pokemonList[randomIndex];
        if (!(pokemon.indexOf(randomPokemon) >=0)){
          pokemon.push({pokemon:randomPokemon, isFlipped:false, isMatched:false});
          pokemon.push({pokemon:randomPokemon, isFlipped:false, isMatched:false});
        }
      }
      pokemon = pokemon.sort(() => Math.random() - 0.5);
      this.pokemonPairsList = pokemon;
    },


    flipTile(element){
      if (!element.isFlipped && this.turnEnd){
        element.isFlipped = !element.isFlipped
        this.checkPair(element)
      }

      else{return}
    },


    async checkPair(element){
      //if 1st card
      if (this.firstTile === null) {
          this.firstTile = element;
          console.log("firstTile")
          return;
      }
      //if 2nd card
      else {
          this.turnEnd = false;
          if (this.firstTile.pokemon.pokedex_id === element.pokemon.pokedex_id) { //match
            console.log("match")
            if (!(this.level==='Facile')){
              console.log("pas facile")
              this.displayMoyenInput = (this.level==='Moyen');
              this.displayDifficileInput = (this.level==='Difficile');
              this.answer = await this.validationInput();
              if (!this.valid()){
                console.log("not valid")
                this.firstTile.isFlipped = false;
                element.isFlipped = false;
                this.firstTile = null;
                this.turnEnd = true;
                this.displayMoyenInput = false;
                this.displayDifficileInput = false;
                this.answer='';
              }
              else{
                console.log("valid")
                this.displayMoyenInput = false;
                this.displayDifficileInput = false;
                this.firstTile.isMatched = true;
                element.isMatched = true;
                this.checkEnd();
                this.firstTile = null;
                this.answer='';
                this.turnEnd = true;
              }
            }
            else{
              console.log("valid")
              this.firstTile.isMatched = true;
              element.isMatched = true;
              this.checkEnd();
              this.firstTile = null;
              this.answer='';
              this.turnEnd = true;
            }
          }
          else { //mismatch
              console.log("mismatch")
              setTimeout(() => {
                  this.firstTile.isFlipped = false;
                  element.isFlipped = false;
                  this.firstTile = null;
                  this.turnEnd = true;
              }, 1000);
          }
      }
    },

    valid(){
      if (this.level==='Moyen'){
        return (this.firstTile.pokemon.name.fr.toLowerCase() === this.answer.toLowerCase())
      }
      if (this.level==='Difficile'){
        return (this.firstTile.pokemon.pokedex_id===parseInt(this.answer))
      }
    },

    validationInput() {
      return new Promise((resolve) => {
        this.resolveValidation = resolve; // Store the resolve function
      });
    },

    sendAnswer() {
      if (this.resolveValidation) {
        this.resolveValidation(this.inputValue); // Resolve the promise with the input value
        this.inputValue = '';
        this.resolveValidation = null; // Reset
      }
    },

    checkEnd(){
      if (this.pokemonPairsList.every(element => element.isMatched)){
        this.gameEnd=true;
      }
    },
  },

  computed: {
    pokemonList(){
      if (this.memoryGeneration==='') return this.pokemonData;
      else return this.pokemonData.filter(pokemon => pokemon.generation === parseInt(this.memoryGeneration));
    },
  },

  mounted() {
    this.choosePokemon();
  },

  watch: {
    displayMoyenInput(newValue) {
      if (newValue) {
          nextTick(() => {
            this.$refs.MoyenInput?.focus();
          });
      }
    },

    displayDifficileInput(newValue) {
      if (newValue) {
          nextTick(() => {
            this.$refs.DifficileInput?.focus();
          });
      }
    },
    
    level(newValue) {
      if (newValue) {
        this.choosePokemon()
      }
    },

    memoryGeneration(newValue) {
      if (newValue) {
        this.choosePokemon()
      }
    },
  },
}

</script>


<template>

    <div class="memory-grid">
      <div v-for="(element, index) in pokemonPairsList" :key="element.pokemon.pokedex_id + '-' + index"
          class="tile"
          :class="{ flip: element.isFlipped, clickable: !element.isFlipped }"
          @click="flipTile(element)" >
        <div class="front"></div>
        <div class="back">
          <img class="img1":src=element.pokemon.sprites.regular />
        </div>
      </div>

      <div class="small-pop" v-if="(displayMoyenInput)">
        <div> Trouvez le nom du pokémon :</div>
        <input id="moyen-input"
              class="validation-input"
              v-model="inputValue"
              placeholder="Entrez un nom"
              @keypress="(e) => {if (e.key === 'Enter'){ sendAnswer() }}"
              ref="MoyenInput">
      </div>

      <div class="small-pop" v-if="(displayDifficileInput)">
        <div> Trouvez le numéro du pokémon :</div>
        <input id="difficile-input"
              class="validation-input"
              v-model="inputValue"
              placeholder="Entrez un nombre"
              @keypress="(e) => {if (e.key === 'Enter'){ sendAnswer() }}"
              ref="DifficileInput">
      </div>

      <div class="big-pop" v-if="gameEnd">
        C'est gagné !
        <button class="restart" @click="gameEnd=false, choosePokemon()"> Rejouer </button>
      </div>

    </div>

</template>


<style>

.memory-grid{
  width:50vw;
  display: grid;
  grid-template:repeat(4,1fr)/repeat(4,1fr);
  row-gap: 1vw;
  column-gap: 1vw;

  @media screen and (max-width: 800px) {
    width:80vw;
  }
}

.big-pop{
  position:absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  height:fit-content;
  width:30vw;
  padding: 1% 0% 1% 0%;
  display:flex;
  flex-direction:column;
  text-align: center;
  justify-content:center;
  align-items: center;
  font-size: 1rem;
  font-weight: bold;
  color:white;
  letter-spacing: 1px;
  border-radius: 20px;
  border: 4px solid rgba(28, 77, 155, 0.889);
  background-color: rgb(100, 142, 240);
  z-index: 10;

  @media screen and (max-width: 800px) {
    width:50vw;
    font-size: 0.8rem;
  }
}

.small-pop{
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  height:fit-content;
  width:30vw;
  position:absolute;
  display:flex;
  flex-direction: column;
  justify-content:center;
  gap:5px;
  padding: 1% 3% 2% 3%;
  border-radius: 5px;
  border: 2px solid rgb(169, 169, 169);
  background-color: rgb(226, 226, 226);
  z-index: 10;

  @media screen and (max-width: 800px) {
    width:60vw;
    font-size: 0.8rem;
    height:fit-content;
    }	
}

.tile{
  position: relative;
  text-align: center;
  border-radius: 5px;
  box-shadow: 0px 0px 10px 1px rgba(6, 38, 109, 0.2);
}

.clickable{
  cursor:pointer;
}

.front {
  position:absolute;
  height:100%;
  width:100%;
  border-radius: 5px;
  backface-visibility: hidden;
  background-color: rgb(81, 95, 138);
}

.back {
  position:relative;
  align-content: center;
  height:100%;
  width:100%;
  border-radius: 5px;
  backface-visibility: hidden;
  transform: rotateY(180deg);
}

.tile.flip {
  transform: rotateY(180deg);
  transition: all 0.5s ease;
  transform-style: preserve-3d;
}

.restart{
  padding: 1% 2% 1% 2%;
  margin: 2%;
  border-radius: 5px;
  background-color: rgb(255, 237, 184);
  border-color: rgb(125, 134, 163);
  font-size: 0.7rem;
  cursor:pointer;
}

.validation-input{
  width:98%;
  height:auto;
  font-size: 1rem;
  border-radius: 5px;
  border-width: 1px;
  padding-left: 2%;

  @media screen and (max-width: 800px) {
    font-size: 0.8rem;
  }
}

</style>