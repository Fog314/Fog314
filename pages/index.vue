<template>
    <popluar-drinks />
</template>
<script lang="ts">

import { defineComponent } from '@nuxtjs/composition-api'
import PopluarDrinks from '@/components/PopluarDrinks.vue'

export default defineComponent({
  components: { PopluarDrinks },
    setup() {
        
    },
})
</script>
<style lang="scss">
    body {
        margin: 0;
    }

    a {
        text-decoration: none;
        color: inherit;
    }
</style>