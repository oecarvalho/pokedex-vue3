<template>
    <div class="card">
        <img src="/src/assets/pokémon.png" alt="">

        <div class="pokemon-info">
            <h3>Pikachu</h3>
            <p>ID: 101</p>
            <div class="pokemon-tipo">
                <span class="poketipo">Elétrico</span>
                <span class="poketipo red">Fogo</span>
            </div>
        </div>

        <button>Ver Detalhes</button>
    </div>
</template>

<script>

</script>


<style>
    .card{
        width: 100%;
        max-width: 12.25rem;
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

    .pokemon-info h3{
        color: white;
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
    }

    span{
        font-size: 14px;
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
        background-color: #FFCB05;
        color: #343232;
        border-radius: 8px;
        border: none;
        padding: 8px 16px;
        font-weight: 500;
        margin-bottom: 14px;
        cursor: pointer;
    }
</style>