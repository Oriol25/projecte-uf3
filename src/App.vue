<template>
    <div>
        <GameControl></GameControl>
    </div>
</template>

<script lang="ts">
    import { defineComponent } from 'vue'
    import GameControl  from './components/GameControl.vue'

    export default defineComponent({
        components: {
            GameControl
        }
    })
</script>

<style>
    @import url('./assets/css/app.css');
    @import url('./assets/partials/base_partials.css');
</style>
