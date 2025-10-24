<script>
export default {

    props: {
        pokemon: Object,
        typeColors: Object,
        filteredData: Array,
        step: Number
    },

    methods: {
        getColor(type) {
            let color = this.typeColors[type]
            return color
        }
    },

    emits: ['nextStep'],

    computed: {
        vertical() {
            return (window.innerWidth < 800)
        },
    }
}
</script>


<template>
    <div class="bloc-general" @click="$emit('nextStep')">

        <img class="img3" v-bind:src=pokemon.sprites.regular />

        <transition name="fade">
            <div class="bloc" v-if="step >= 2">
                <p> <b>{{ pokemon.name.fr }}</b> </p>
                <p> # {{ pokemon.pokedex_id }}</p>
            </div>
        </transition>

        <transition name="fade">
            <div class="type-list transition" v-if="step === 3">

                <div class="type" :style="{ backgroundColor: getColor(pokemon.types[0].name) }">
                    {{ pokemon.types[0].name }}
                </div>
                <div class="type" v-if="pokemon.types[1]" :style="{ backgroundColor: getColor(pokemon.types[1].name) }">
                    {{ pokemon.types[1].name }}
                </div>
            </div>
        </transition>
    </div>


</template>

<style lang="css">
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease, transform 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
    transform: translateY(20px);
}

.img3 {
    max-width: 30vh;
    height: auto;
    aspect-ratio: 1/1;

    @media screen and (max-width: 800px) {
        max-width: 20vh;
        min-width: 130px;
        ;
    }
}

.bloc-general {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 1vh;
    align-items: center;
    justify-content: center;
    font-family: sans-serif;
}


.bloc {
    height: fit-content;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.type-list {
    display: flex;
    gap: 10%;
    justify-content: center;
}

.type {
    padding: 7px 14px 7px 14px;
    border-radius: 15px;
    text-align: center;

    @media screen and (max-width: 800px) {
        width: 50%;
    }
}


.title {
    color: rgb(174, 174, 174);
    font-weight: 600;
    font-size: 1.2rem;

    @media screen and (max-width: 800px) {
        font-size: 1.1rem;
        width: 100%;
    }
}

p {
    padding-top: 0px;
    margin: 0px;

    @media screen and (max-width: 800px) {
        font-size: 0.9rem;
    }
}

.bloc p {
    display: inline-block;
}
</style>