<script>
export default {

data(){
    return {
        searchQuery:'',
        type1Query:'',
        type2Query:'',
        generation:'',
        memoryGeneration:'',
        selectedLevel:'Facile',
    }
},

  props: {
    componentDisplayed:String,
    typeColors:Object,
    filteredData:Array,
  },

  emits: ['toggleDisplay','updateKey','updateType1','updateType2', 'updateGen', 'updateMemGen','updateLevel'],

  methods:{
    resetTypes(){
      this.type1Query=''
      this.type2Query=''
    },

    resetSearch(){
      this.searchQuery=''
    },

    resetGen(){
      this.generation=''
    },

    resetMemGen(){
      this.memoryGeneration=''
    },

    updateLevel(level){
      this.selectedLevel=level
    },

  },

  computed: {
    numberResults() {
      return this.filteredData.length
    },
  },

}
</script>

<template>

<div class="menu">

    <div class="logo">
        <img src="../assets/ronflex.png" alt="logo" class="img-logo">
        <p class="title-logo">My POKédex</p>
    </div>

    <div class="bar-item" :class="{ activeBar: componentDisplayed==='pokedex_cards'}" @click="$emit('toggleDisplay','','pokedex_cards')">
        Accueil - pokédex
    </div>

    <div class="bar-item" :class="{ activeBar: componentDisplayed==='memory'}" @click="$emit('toggleDisplay','','memory')">
        Mémory
    </div>

    <div class="bar-item">
        <i class="fa-regular fa-star"></i>
        <p> Favoris </p>
    </div>

    <div v-if="componentDisplayed=='pokedex_cards'" class="filter-list">
        <div class="title-section">Filtrer le pokédex :</div>
        <div class="filter-layout">
            <div class="filter-item">
                <input id="research"
                        class="research"
                        v-model="searchQuery"
                        placeholder="Rechercher nom ou ID"
                        @input="$emit('updateKey', searchQuery)">

                <i v-if="searchQuery" class="fa-solid fa-square-xmark"  style="color: #858585; cursor:pointer;" @click="resetSearch(); $emit('updateKey', searchQuery)"></i>
            </div>
        </div>

        <div class="filter-layout">
            <div class="title-choice">Choisir un ou deux types :</div>

            <div class="filter-item">
                <select class="select-type" id="type1" v-model="type1Query" @change="$emit('updateType1', type1Query)">
                    <option disabled value="">Choisir un type</option>
                    <option v-for="option in Object.keys(typeColors)">
                        {{ option }}
                    </option>
                </select>

                <select class="select-type" id="type2" v-model="type2Query" @change="$emit('updateType2', type2Query)">
                    <option disabled value="">Choisir un type</option>
                    <option v-for="option in Object.keys(typeColors)">
                        {{ option }}
                    </option>
                </select>

                <i v-if="(type1Query || type2Query)"
                    class="fa-solid fa-square-xmark"
                    style="color: #858585; cursor:pointer;"
                    @click="resetTypes(); $emit('updateType1', type1Query); $emit('updateType2', type2Query) "></i>
            </div>
        </div>

        <div class="filter-layout">
            <div class="title-choice">Choisir une génération :</div>

            <div class="filter-item">
                <select class="select-type" id="gen" v-model="generation" @change="$emit('updateGen', generation)">
                    <option disabled value=""> Toutes </option>
                    <option v-for="number in [1,2,3,4,5,6,7,8,9]">
                        {{ number }}
                    </option>
                </select>

                <i v-if="(generation)"
                    class="fa-solid fa-square-xmark"
                    style="color: #858585; cursor:pointer;"
                    @click="resetGen(); $emit('updateGen', generation)"></i>
            </div>
        </div>

        <p class="results"> Résultats affichés :  {{ numberResults }}</p>

    </div>


    <div v-if="componentDisplayed=='memory'" class="filter-list">

        <div class="title-section">Paramètres Mémory</div>

        <div class="filter-layout">
            <div class="title-choice">Choisir un niveau de difficulté :</div>
            <div class="filter-item">
                <button class="level-button" 
                        :class="{ selected: selectedLevel === 'Facile' }"
                        @click="updateLevel('Facile'), $emit('updateLevel', selectedLevel)">
                    Facile 
                </button>
                <button class="level-button" 
                        :class="{ selected: selectedLevel === 'Moyen' }"
                        @click="updateLevel('Moyen'), $emit('updateLevel', selectedLevel)">
                    Moyen 
                </button>
                <button class="level-button" 
                        :class="{ selected: selectedLevel === 'Difficile' }"
                        @click="updateLevel('Difficile'), $emit('updateLevel', selectedLevel)">
                    Difficile 
                </button>
            </div>
        </div>

        <div class="filter-layout">
            <div class="title-choice">Choisir une génération :</div>
            <div class="filter-item">
                <select class="select-type" id="gen2" v-model="memoryGeneration" @change="$emit('updateMemGen', memoryGeneration)">
                    <option disabled value=""> Toutes </option>
                    <option v-for="number in [1,2,3,4,5,6,7,8,9]">
                        {{ number }}
                    </option>
                </select>
                <i v-if="(memoryGeneration)"
                    class="fa-solid fa-square-xmark"
                    style="color: #858585; cursor:pointer;"
                    @click="resetMemGen(); $emit('updateMemGen', generation)"></i>
            </div>
        </div>
    </div>
</div>

</template>


<style>

.menu{
    position:fixed;
    top:0px;
    left:0px;
    width:25vw;
    height:100%;
    background-color: rgb(236, 236, 236);
    border-right: 1px solid rgb(179, 179, 179);
    display:flex;
    flex-direction: column;
    z-index: 10;
}

.logo{
    display:flex;
    justify-content: center;
    align-items: center;
    padding:10% 0% 10% 0%;
    border-bottom: 1px solid rgb(179, 179, 179);
}

.img-logo{
    width:15%;
    height:auto;
}

.title-logo{
    font-size: 2rem;
    font-weight: bold;
}

.bar-item{
    width:80%;
    padding: 5% 10% 5% 10%;
    display:flex;
    align-items: center;
    justify-content: baseline;
    border-bottom: 1px solid rgb(179, 179, 179);
    cursor:pointer;
}

.bar-item:hover{
    font-weight: 600;
    background-color: rgb(218, 218, 218);
}

.activeBar{
    pointer-events: none;
    font-weight: 600;
    background-color: rgb(218, 218, 218);
}

.filter-list{ 
    display:flex;
    flex-direction: column;
    gap:2vh;
    /* haut droite bas gauche */
    padding-top: 5% 
}

.filter-item{
    width:80%;
    /* haut droite bas gauche */
    padding: 2% 0% 2% 10%;
    display:flex;
    align-items: center;
    justify-content: flex-start;
    gap:2%;
}

.filter-layout{
    width:100%;
    display:flex;
    flex-direction: column;
    justify-content: center;
}

.title-choice{
    /* haut droite bas gauche */
    padding: 0% 0% 0% 10%;
}

.title-section{
    /* haut droite bas gauche */
    padding: 5% 0% 3% 10%;
    font-weight:1000;
    letter-spacing: 0.5px;
    color: rgb(160, 160, 160);
}

.select-type{
    height:2rem;
    width:45%;
    font-size: 0.7rem;
    color: rgb(114, 114, 114);
    border-radius: 5px;
    border-color: rgb(184, 184, 184);
    border-width: 2px;
}

.research{
    width:88%;
    height:2rem;
    padding-left: 3%;
    border-radius: 5px;
    border-color: rgb(211, 211, 211);
}

.results{
    padding: 0% 0% 0% 10%;
    color:rgb(168, 168, 168);
    font-weight:500;
    font-style: italic;
}

.level-button{
    height:2rem;
    width:45%;
    border-radius: 5px;
    border-color: rgb(184, 184, 184);
    cursor:pointer;
}

.level-button:hover, .level-button.selected{
    border-color: rgb(58, 88, 163);
    background-color: rgb(213, 221, 232);
    color: rgb(37, 55, 100);
    font-weight: 600;
}

/* 
.moyen{
    height:2rem;
    width:45%;
    border-radius: 5px;
    border-color: rgb(184, 184, 184);
    cursor:pointer;
}

.moyen:hover{
    background-color: rgb(237, 254, 152);
    border-color: rgb(150, 161, 96);
    font-weight: 600;
}

.moyen:active, moyen.pushed{
    background-color: rgb(244, 255, 189);
    border-color: rgb(197, 211, 127);
    font-weight: 600;
}

.difficile{
    height:2rem;
    width:45%;
    border-radius: 5px;
    border-color: rgb(184, 184, 184);
    cursor:pointer;
}

.difficile:hover{
    background-color: rgb(254, 177, 141);
    border-color: rgb(172, 118, 92);
    font-weight: 600;
}

.difficile:active, difficile.pushed{
    background-color: rgb(255, 196, 169);
    border-color: rgb(213, 145, 113);
    font-weight: 600;
} */

</style>