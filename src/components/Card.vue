<template>
    <div class="pokemon-card">        
        <h3>{{ pokemon.name }}</h3>
        <div class="pokemon-inf">
            <p>Height: {{ pokemon.height/100 }} Meters</p>
            <p>Weight: {{ pokemon.weight }} Kg</p>
            <p>Type: {{ generateTypeTextPokemon(pokemon.type)  }}</p>
        </div>

        <img :src="resolveImg(pokemon)">
    </div>
</template>

<script>
export default {
    name: 'Card',
    
    props: {
        pokemon: {
            type: Object,
            required: true
        }
    },

    methods: {
        generateTypeTextPokemon(types) {
            const length = types.length
            return types.reduce((acc, type,index) => {
                acc += `${type}`
                if (index < length -1) acc += `, `                
                return acc
            }, '')
        },

        resolveImg(pokemon) {
            const imgUrl = pokemon.sprites.other["official-artwork"]["front_default"]
            return imgUrl
        },
    }
}
</script>

<style lang="scss" scoped>
.pokemon-card {
    position:relative;
    display: grid;
    width: 300px;
    height: 300px;
    grid-template-columns: 100%;
    grid-template-rows: 60% 10% 30%; 
    border-radius: 20px;
    background-image: url(https://i.pinimg.com/564x/98/c1/5a/98c15a449a1166ec23f5c9f1f63995dd.jpg);
    
    h3 {
        transform: translateY(20px);
        font-size: 36px;
        grid-row-start: 2;
        align-self: center;
    }
    img {
        width: 80%;
        height: 80%;
        grid-column-start: 1;
        grid-row-start: 1;
        justify-self: center;
        align-self: center;
        
    }
    .pokemon-inf{
        transform: translateY(50px);
        grid-row-start: 3;        
    }
    p {
        opacity:0;
        margin-bottom: 2px;
        margin-top: 2px;
    }
    
}
.pokemon-card:hover {
    .pokemon-inf {
        transition: 1s;
        transform: translateY(0px);
    }
    p {
        opacity:1;
        transition-duration: 2s;
        
    }
    h3 {
        font-size: 24px;
        transform: translateY(0px);
        transition-duration: 1s;
    }
    img {
        transition: translateY(0px);
        transition-duration: 1s;
        width: 90%;
        height: 90%;
    }
    
}
    
</style>