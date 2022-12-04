<template>
    <div class="about">
        <div v-if="(isFound === false)" class="mx-auto mt-12 flex justify-center">
            <span>Pokemon Not Found</span>
        </div>
        
        <div v-else>
            <div
            v-if="pokemon"
            className="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
            >
                <h3 className="text-2xl text-green-900 uppercase">{{ pokemon.name }}</h3>
                <div class="flex justify-center">
                    <img className="w-48" :src="pokemon.sprites.front_shiny" alt="" />
                    <img className="w-48" :src="pokemon.sprites.back_shiny" alt="" />
                </div>
                <h3 class="text-yellow-400">Types</h3>
                <div v-for="(pkmn, idx) in pokemon.types" :key="idx">
                    <h5 class="text-blue-900">{{pkmn.type.name}}</h5>
                </div>
            </div>
        </div>

            <div class="p-14 flex justify-center">
                <router-link to="/"
                class="text-white 
                bg-purple-700 
                hover:bg-purple-800 
                focus:ring-4 
                focus:ring-blue-300 
                font-medium 
                rounded-lg 
                text-sm 
                px-5 
                py-2.5 
                text-center 
                mr-2 
                mb-2 
                dark:bg-purple-400 
                dark:hover:bg-purple-700 
                dark:focus:ring-purple-800">
                Return to Picker
                </router-link>
            </div>
        </div>
    
</template>
  
<script setup>
import { onMounted, ref } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const pokemon = ref(null)
const isFound = ref(true)

const fetchPkmn = async () => {
    try {
    await fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
    .then((res) => res.json())
    .then((data) => {
        pokemon.value = data;
    });
    } catch (err) {
        isFound.value = false
        console.log(err)
    }
}

onMounted(() => {
    fetchPkmn()
})
</script>
  