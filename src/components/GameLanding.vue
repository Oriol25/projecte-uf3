<template>
    <div class="content" id="content">
        <!-- NAV -->
        <header class="nav-game">
            <!-- STATISTICS -->
            <a href="#"><i class="fa-solid fa-chart-column fa-2xl"></i></a>
            <!-- TITLE -->
            <h3>WORDLE</h3>
            <!-- RELOAD GAME & HELP -->
            <div class="buttons">
                <a href="#"><i class="fa-solid fa-arrows-rotate fa-2xl"></i></a>
                <a href="#"><i class="fa-sharp fa-solid fa-question fa-2xl"></i></a>
            </div>
        </header>

        <!-- GAME -->
        <div class="game">
            <div v-for="(row, index) in rowletters" :key="index">
                <Row :data="row"/>
            </div>
        </div>

        <!-- KEYWORD -->
        <div>
            
        </div>
     </div>
</template>

<script lang="ts">
    import { Vue, Options } from 'vue-class-component';
    import { Prop } from 'vue-property-decorator';
    import Row from './Row.vue'

    type Letter = {
        letter: String,
        status: String
    }

    type Row = Letter[]

    @Options({
        components: {
            Row
        }
    })
    export default class GameLanding extends Vue {
        
        @Prop() readonly rowletters!: Row
        
        created() {

        }
        
    }
</script>

<style scoped>
    .game {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 1em;
        gap: 0.5em;
    }
</style>