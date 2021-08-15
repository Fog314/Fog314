<template>
    <div class="drink-page">
        <div class="drink-page__img-wrapper">
            <img
                :src="drink.strDrinkThumb"
                class="drink-page__img"
            >
        </div>
        <div class="drink-page__header">
            <div
                class="drink-page__title"
                v-text="drink.strDrink"
            />
            <div class="drink-page__type" v-text="drink.strAlcoholic" />
            <div class="drink-page__type" v-text="drink.strCategory" />
        </div>
        <div class="drink-page__body">
            <div class="drink-page__ingredients">
                <div class="drink-page__ingredients-left">
                    <div
                        v-for="(item, index) in ingredients.filter(el => el.ingredient)"
                        :key="index"
                        class="drink-page__ingredients-item ingredient"
                    >
                        <div class="ingredient__name">{{ item.ingredient }}</div>
                    </div>
                </div>
                <div class="drink-page__ingredients-right">
                    <div
                        v-for="(item, index) in ingredients.filter(el => el.measure)"
                        :key="index"
                        class="drink-page__ingredients-item ingredient"
                    >
                        <div class="ingredient__name">{{ item.measure }}</div>
                    </div>
                </div>
            </div>
            <div
                class="drink-page__text"
                v-text="drink.strInstructions"
            />
        </div>
    </div>
</template>

<script lang="ts">
    import { defineComponent, useContext, ref, onMounted } from '@nuxtjs/composition-api'
    import axios from "axios"

    export default defineComponent({
        name: 'DrinkPage',
        props: {
            strDrink: {
                type: String,
                default: null
            },
            strDrinkThumb: {
                type: String,
                default: null
            }
        },
        setup(props) {
            const { route } = useContext()
            const drink = ref({})
            const ingredients = ref([])

            const i = route.value.params.drinkName

            const fetchData = (async () => {
                const options = {
                    method: 'GET',
                    url: 'https://the-cocktail-db.p.rapidapi.com/lookup.php',
                    params: { i },
                    headers: {
                        'x-rapidapi-key': '00b52db9b6msha80f9b5af190c45p123de7jsnc56f628c8dc7',
                        'x-rapidapi-host': 'the-cocktail-db.p.rapidapi.com'
                    }
                }

                const { data } = await axios.request(options)

                if (!data) {
                    return undefined
                }

                drink.value = data.drinks[0]

                for (const [key, value] of Object.entries(data.drinks[0])) {
                    if (key.includes('strIngredient') && value) {
                        ingredients.value.push({ ingredient: value })
                    } else if (key.includes('strMeasure') && value) {
                        ingredients.value.push({ measure: value })
                    }
                }

                // const firstMeasure = ingredients.value.findIndex((e)=>{ return e.measure })
            })

            onMounted (async () => {
                await fetchData()
            })

            return {
                drink,
                ingredients
            }
        },
    })
</script>
<style lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Anton&family=Montserrat&display=swap');

    body {
        margin: 0;
    }

    .drink-page {
        display: flex;
        flex-direction: column;
        font-family: Anton;
        margin: auto;
        max-width: 480px;

        &__img {
            width: 100%;
            height: 150px;
            object-fit: cover;
            object-position: top;
        }

        &__img-wrapper {
            overflow: hidden;
        }

        &__title {
            font-size: 24px;
        }

        &__type {
            color: #bdbdbd;
            display: inline;
        }

        &__header {
            padding: 10px 20px 10px;
            border-bottom: 1px solid #bdbdbd;
        }

        &__body {
            font-family: 'Montserrat';
            font-size: 20px;
            padding: 10px 20px 10px;
            text-align: justify;
        }

        &__ingredients {
            display: flex;
            padding-bottom: 20px;
            gap: 20px;
            font-size: 18px;
        }

        &__ingredients-left {
            display: flex;
            flex-direction: column;
            font-weight: bold;
            gap: 10px
        }

        &__ingredients-right {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
    }
</style>
