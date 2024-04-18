<script setup>
import axios from "axios";
import { onMounted, reactive, ref, computed } from 'vue';

const pokeCards = reactive(ref([]));

onMounted(async ()=>{ 
    try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0');
        const pokemons = response.data.results;

        await Promise.all(
            pokemons.map(async (pokemon) => {
                const urlDetalhes = pokemon.url; //acessando a url de detalhes dos pokémons
                const pokeDados = await axios.get(urlDetalhes);
                const { name, id, sprites, types } = pokeDados.data;

                const pokeCard = {//objeto com os dados necessários do pokémon
                    nome: name,
                    id: id,
                    imagem: sprites.front_default,
                    tipo: types[0].type.name
                };
                pokeCards.value.push(pokeCard);
            })
        );
    } catch (error) {
        console.error('Erro ao listar os pokémons:', error);
    }

}) 

let campoDeBusca = ref('');

const filtroPokemon = computed(() => {
    if (pokeCards.value && campoDeBusca.value) {
        const busca = campoDeBusca.value.toLowerCase().trim();
        if (!isNaN(busca)) {
            return pokeCards.value.filter(pokemon =>
                pokemon.id.toString().includes(busca)
            );
        } else {
            return pokeCards.value.filter(pokemon =>
                pokemon.nome.toLowerCase().includes(busca) ||
                pokemon.tipo.toLowerCase().includes(busca)
            );
        }
    }
    return pokeCards.value;
});

</script>

<template>
    <div class="conteudo-principal">

        <input type="text" placeholder="Digite o nome, id ou o tipo do Pokémon" v-model="campoDeBusca">

        <ul class="pokemon-list">
            <li v-for="pokemon in filtroPokemon" :key="pokemon.id" class="card">
                <img :src="pokemon.imagem" :alt="'este pokémon é o: ' + pokemon.nome" /> 

                <div class="pokemon-info">  
                        <h4>{{ pokemon.nome }}</h4> 
                        <p>ID: {{ pokemon.id }}</p>  

                        <div class="pokemon-tipo">
                            <span class="poketipo">  
                                {{pokemon.tipo}}
                            </span>
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
        display: grid;
        grid-template-columns:  repeat(auto-fit, minmax(220px, 1fr));
        gap: 16px;
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
        width: 100%;
        max-width: 100px;
        height: 100px;
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
        background-color: #B3B3B3;
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
        max-width: 1274px;
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