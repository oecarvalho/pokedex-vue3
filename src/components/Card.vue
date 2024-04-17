<script setup>
import { onMounted, reactive, ref, computed } from 'vue';


let pokemons = reactive(ref([]));
let urlSprites = ref('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/');
let campoDeBusca = ref('');

onMounted( async () => {
    try{
        const resposta = await fetch('https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0');
        const dados = await resposta.json();
        pokemons.value = dados.results;
    } catch (error){
        console.error("erro na busca de pokémons:", error);
    }
})


const filtroPokemons = computed(()=>{
    if(pokemons.value && campoDeBusca.value){
        return pokemons.value.filter(pokemon => 
            pokemon.name.toLowerCase().includes(campoDeBusca.value.toLocaleLowerCase())
        )
    }
    return pokemons.value;
})
</script>


<template>

    <div class="conteudo-principal">

        <input type="text" placeholder="Digite o nome ou id do Pokémon" v-model="campoDeBusca">

        <ul>
            <li class="card" v-for="pokemon in filtroPokemons" :key="pokemon.name">
                
                <img :src="urlSprites + pokemon.url.split('/')[6] + '.png'" alt="">  

                <div class="pokemon-info">
                    <h4>{{ pokemon.name }}</h4> 
                    <p>ID: {{ pokemon.url.split('/')[6] }}</p>
                    <div class="pokemon-tipo"> 
                        <span class="poketipo">Elétrico</span>
                        <span class="poketipo red">Fogo</span>
                    </div>
                </div>

                <button>Ver Detalhes</button>
            </li>
        </ul>
    </div>
</template>


<style>
    .conteudo-principal{
        display: flex;
        flex-direction: column;
    }

    .conteudo-principal ul{
        display: flex;
        flex-wrap: wrap;
        gap: .75rem;
    }

    .card{
        width: 100%;
        max-width: 16.25rem;
        background-color: #4B4B4D;
        display: flex;
        flex-direction: column;
        align-items: center;
        border-radius: 8px;
    }

    .card img{
        margin-top: 28px;
        margin-bottom: 8px;
    }

    .pokemon-info{
        text-align: center;
    }

    .pokemon-info h4{
        color: white;
        text-align: center;
        
    }

    p{
        font-size: 12px;
        color: #D2D4D6;
        margin-bottom: 12px;
    }
    .pokemon-tipo{
        display: flex;
        gap: 8px;
        align-items: center;
        margin-bottom: 8px;
        justify-content: center;
    }

    span{
        font-size: 12px;
        color: white;
    }

    .poketipo{
        background-color: #FFCB05;
        border-radius: 8px;
        color: #000;
        padding: 4px 8px;
        margin-bottom: 12px;
    }

    .red{
        background-color: red;
        color: #fff;
    }

    button{
        width: 100%;
        max-width: 220px;
        background-color: #FFCB05;
        color: #343232;
        border-radius: 8px;
        border: none;
        padding: 8px 16px;
        font-weight: 500;
        margin-bottom: 14px;
        cursor: pointer;
    }

    input{ 
        width: 100%;
        max-width: 1050px;
        margin-top: 1.5rem;
        background-color: #4B4B4D;
        height: 24px;
        padding: 1rem;
        border-radius: .5rem;
        color: white;
        background-image: url('/src/assets/search.svg');
        background-repeat: no-repeat;
        background-position: center right 1.25rem;
        border: none;
        margin-bottom: 36px;
    }

    input::placeholder{
        color: white;
        
    }

    input:focus{
        outline: 0;
        box-shadow: none;
        border: none;
    }

</style>