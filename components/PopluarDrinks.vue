<template>
    <div class="popular-drinks">
        <div class="popular-drinks__title">
            Popular cocktails
        </div>
        <div class="popular-drinks__wrapper">
            <DrinkCard
                v-for="item in popularDrinks"
                :key="item.idDrink"
                v-bind="item"
            />
        </div>
    </div>
</template>

<script lang="ts">
    import { defineComponent, ref, onMounted } from '@nuxtjs/composition-api'
    import axios from "axios";
    import DrinkCard from "@/components/DrinkCard.vue"

    export default defineComponent({
        name: 'PopularDrinks',
        components: { DrinkCard },
        setup() {
            const popularDrinks = ref([{}])

            const fetchPopularCocktails = (async () => {
                const options = {
                    method: 'GET',
                    url: 'https://the-cocktail-db.p.rapidapi.com/popular.php',
                    params: {i: 'vodka'},
                    headers: {
                        'x-rapidapi-key': '00b52db9b6msha80f9b5af190c45p123de7jsnc56f628c8dc7',
                        'x-rapidapi-host': 'the-cocktail-db.p.rapidapi.com'
                    }
                }

                const { data } = await axios.request(options)

                if (!data) {
                    return undefined
                }

                popularDrinks.value = data.drinks
            })

            onMounted (() => {
                fetchPopularCocktails()
            })

            return {
                popularDrinks,
                fetchPopularCocktails
            }
        },
    })
    
</script>
<style lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Anton&display=swap');

    .popular-drinks {
        font-size: 32px;
        padding-bottom: 20px;
        max-width: 320px;
        margin: auto;

        &__title {
            font-family: 'Anton';
        }

        &__wrapper {
            display: flex;
            flex-direction: column;
            gap: 40px;
        }
    }
</style>
