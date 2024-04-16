<script>
    export default {
        data(){
            return{
                menu: [
                    {imagemUrl: './src/assets/pokeball.svg', nome: 'Todos'},
                    {imagemUrl: './src/assets/ghost.svg', nome: 'Fantasma'},
                    {imagemUrl: './src/assets/stone.svg', nome: 'Pedra'},
                    {imagemUrl: './src/assets/plants.svg', nome: 'Planta'},
                    {imagemUrl: './src/assets/dragon.svg', nome: 'Dragão'},
                    {imagemUrl: './src/assets/metal.svg', nome: 'Metal'},
                    {imagemUrl: './src/assets/shock.svg', nome: 'Elétrico'},
                    {imagemUrl: './src/assets/water.svg', nome: 'Água'},
                    {imagemUrl: './src/assets/fire.svg', nome: 'Fogo'},
                    {imagemUrl: './src/assets/bug.svg', nome: 'Inseto'},
                    {imagemUrl: './src/assets/flying.svg', nome: 'Voador'},
                    
                ]
            }
        }
    }
</script>


<template>
    <div class="container">
        <ul>
            <li v-for="item in menu" :key="item">
                <img :src="item.imagemUrl" alt="icone do elemento">
                <a href="#">{{ item.nome }}</a>
            </li>
        </ul>
    </div>
</template>


<style scoped>
    ul{
        margin-top: 2rem;
    }

    ul li{
        display: flex;
        align-items: center;
        gap: 1.375rem;
        margin-bottom: 1.75rem;
    }

    a{
        font-size: 14px;
        color: white;
    }

    img{
        max-width: 18px;
        width: 100%;
    }
</style>