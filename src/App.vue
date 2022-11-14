<template>
    <div>
        <GameControl></GameControl>
    </div>
</template>

<script lang="ts">
    import { Options, Vue } from 'vue-class-component'
    import GameControl  from './components/GameControl.vue'

    @Options({
        components: {
            GameControl
        }
    })
    export default class App extends Vue {
            
    }
</script>

<style>
    @import url('./assets/css/app.css');
    @import url('./assets/partials/base_partials.css');
</style>
