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
                const { name, id, sprites, types, game_indices, abilities, stats } = pokeDados.data;

                const tipos = types.map(type => type.type.name);
                const game = game_indices.map(games => games.version.name);
                const habilidades = abilities.map(ability => ability.ability.name)

                const pokeStatus = stats.map(stats => stats.base_stat)
                
                //objeto com os dados necessários do pokémon
                const pokeCard = {
                    indice: pokemons.indexOf(pokemon),
                    nome: name,
                    id: id,
                    imagem: sprites.front_default,
                    tipos: tipos,
                    games: game,
                    poderes: habilidades,
                    evolucoes: [],
                    status: pokeStatus
                };

                const chainResponse = await axios.get(pokeDados.data.species.url);
                const evolutionChain = chainResponse.data.evolution_chain.url;
                const evolutionData = await axios.get(evolutionChain);
                const evolutions = evolutionData.data.chain;

                const getEvolutions = (chain) => {
                    const evolutions = [];
                    let current = chain;
                    while (current && current.species) {
                        evolutions.push(current.species.name);
                        current = current.evolves_to[0];
                    }
                    return evolutions;
                };

                pokeCard.evolucoes = getEvolutions(evolutions);

                pokeCards.value.push(pokeCard);
            })
        );
        pokeCards.value.sort((a, b) => a.indice - b.indice);
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
                pokemon.tipos.some(tipo => tipo.toLowerCase().includes(busca))
            );
        }
    }
    return pokeCards.value;
});

let modalAberto = ref(false);
let pokemonSelecionado = reactive({});

const abrirModal = (pokemon) => {
    pokemonSelecionado = pokemon;
    modalAberto.value = true;
};

const fecharModal = () => {
    modalAberto.value = false;
};

const calcularWidth = (valor) => {
  return valor + '%';
};

const larguraStatus = computed(() => {
  return {
    width: calcularWidth(pokemonSelecionado.status[0]),
  };
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
                            <span v-for="tipo in pokemon.tipos" :key="tipo" class="poketipo">{{ tipo }}</span>
                        </div>
                </div>

                <button @click="abrirModal(pokemon)">Ver Detalhes</button>
            </li>
        </ul>

        <div class="modal" v-if="modalAberto">
            <div class="modal-conteudo">
                <div class="info-geral">

                    <div class="poke-info">
                        <img :src="pokemonSelecionado.imagem" :alt="'este pokémon é o: ' + pokemonSelecionado.imagem" /> 
                        <h3>{{ pokemonSelecionado.nome }}</h3>
                        <p class="id-modal">ID: {{ pokemonSelecionado.id }}</p>

                        <div class="pokemon-tipo-modal">
                            <span v-for="tipo in pokemonSelecionado.tipos" :key="tipo" class="poketipo">{{ tipo }}</span>
                        </div>
                    </div>
                
                    <div class="info-pokemons-modal">
                        <div class="seletores">
                            <div>
                                <h6>Presente nos jogos:</h6>
                                <select id="">
                                    <option v-for="game in pokemonSelecionado.games" :key="game">{{ game }}</option>
                                </select>
                            </div>

                            <div>
                                <h6>Habilidades:</h6>
                                <select id="">
                                    <option v-for="habilidade in pokemonSelecionado.poderes" :key="habilidade">{{ habilidade }}</option>
                                </select>
                            </div>

                           <div>
                            <h6>Evoluções:</h6>
                                <select>
                                    <option v-for="(evolucao, index) in pokemonSelecionado.evolucoes" :key="index">{{ evolucao }}</option>
                                </select>
                           </div>
                    </div>

                    <div class="status">
                        <h6>Status</h6>
                        <ul>
                            <li>
                                <p>Hp:</p>
                                <span v-bind:style="{ width: calcularWidth(pokemonSelecionado.status[0]) }"></span>
                            </li>
                            <li>
                                <p>Attack:</p>
                                <span v-bind:style="{ width: calcularWidth(pokemonSelecionado.status[1]) }"></span>
                            </li>
                            <li>
                                <p>Defense:</p>
                                <span v-bind:style="{ width: calcularWidth(pokemonSelecionado.status[2]) }"></span>
                            </li>
                            <li>
                                <p>Special Attack:</p>
                                <span v-bind:style="{ width: calcularWidth(pokemonSelecionado.status[3]) }"></span>
                            </li>
                            <li>
                                <p>Speed:</p>
                                <span v-bind:style="{ width: calcularWidth(pokemonSelecionado.status[4]) }"></span>
                            </li>
                        </ul>
                    </div>
                    </div>


                </div>
                <button class="btn-modal" @click="fecharModal">Fechar</button>
            </div>
        </div>
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

    .modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }

  .modal-conteudo {
    width: 100%;
    max-width: 600px;
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .info-geral{
    display: flex;
    gap: 1rem;
  }

  .seletores{
    display: flex;
    gap: 32px;
  }

  .poke-info{
    width: 100%;
    max-width: 130px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .pokemon-tipo-modal{
    display: flex;
    gap: 8px;
  }

  .btn-modal{
    max-width: 100%;
  }

  .id-modal{
    font-size: 14px;
    color: #343232;
  }

  .info-pokemons-modal{
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin-bottom: 20px;
  }


  .status ul li{
    display: flex;
    gap: .5rem;
    align-items: center;
  }

  .status ul li p{
    margin-bottom: 0;
    color: #4B4B4D;
    width: 60px;
  }

  .status ul li span{
    display: block;
    background-color: red;
    width: 100%;
    height: 8px;
  }
</style>