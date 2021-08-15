<template>
    <nuxt-link :to="`/drink/${idDrink}`" class="drink-card">
        <div class="drink-card__img-wrapper">
            <img
                :src="strDrinkThumb"
                class="drink-card__img"
            >
        </div>
        <div
            class="drink-card__title"
            v-text="strDrink"
        />
    </nuxt-link>
</template>

<script lang="ts">
    import { defineComponent } from '@nuxtjs/composition-api'

    export default defineComponent({
        name: 'DrinkCard',
        props: {
            idDrink: {
                type: String,
                default: null
            },
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
            
        },
    })
</script>
<style lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Anton&display=swap');

    .drink-card {
        display: flex;
        flex-direction: column;
        font-family: Anton;
        max-width: 320px;
        margin: auto;

        &__img {
            width: 100%;
            border-radius: 4px;
            object-fit: cover;
        }

        &__img-wrapper {
            overflow: hidden;
        }

        &__title {
            font-size: 24px;
        }
    }
</style>
