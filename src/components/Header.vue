<template>
    <header>
        <div class="container">
           <div class="header-content">
                <img src="/src/assets/pokelogo.svg" alt="">
                <ul>
                    <li><a href="#">Português BR</a></li>
                    <li><a href="#">Espanhol</a></li>
                    <li><a href="#">Inglês</a></li>
                </ul>
           </div>
        </div>
    </header>
</template>


<script>
    
</script>

<style scoped>
    header{
        width: 100%;
        background-color: #4B4B4D;
    }

    .header-content{
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1rem 0;
    }

    .header-content ul{
        display: flex;
        gap: 1rem;
        align-items: center;
    }

    .header-content ul li a{
        color: white;
        font-size: .875rem;
    }

</style>
