<template>
    <div class="flex min-h-full items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
        <div class="w-full max-w-md space-y-8">
            <form class="mt-8 space-y-6" action="#" method="POST">
                <input type="hidden" name="remember" value="true" />
                <div class="-space-y-px rounded-md shadow-sm">
                    <div>
                        <input id="pokemon-name" v-model="text" class="relative block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm" placeholder="Enter Pokemon Here..." />
                    </div>
                </div>

                <div class="items-center">
                    <div
                    class="flex flex-wrap ml-4 text-2xl text-blue-400"
                    v-for="(pokemon, idx) in filteredPokemon"
                    :key="idx"
                    >
                        <router-link :to="`/about/${getPokemonId(pokemon.name)}`">
                            {{ pokemon.name }}
                        </router-link>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>

<script setup>
import { computed, ref, onMounted } from "vue";
import { useRouter } from 'vue-router'

const router = useRouter()
const pokemons = ref([])
const text = ref("")

fetch('https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0')
.then((res) => res.json())
.then((data) => {
    pokemons.value = data.results
})

const getPokemonId = (item) => {
    return pokemons.value.findIndex((p) => p.name === item) + 1;
};

const filteredPokemon = computed(() => {
    if(!text.value){
        return [] 
    }
    return pokemons.value.filter((pokemon)=> 
        pokemon.name.includes(text.value)
    )
})
</script>
  