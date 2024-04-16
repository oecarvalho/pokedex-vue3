<template>

    <input type="text" placeholder="Digite o nome ou id do Pokémon">
    
</template>

<script>

</script>

<style scoped>
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