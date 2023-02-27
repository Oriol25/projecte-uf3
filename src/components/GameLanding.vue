<template>
    <div class="content" id="content">
        <!-- NAV -->
        <header class="nav-game">
            <!-- STATISTICS -->
            <span><i class="fa-solid fa-chart-column fa-2xl"></i></span>
            <!-- TITLE -->
            <h3>{{title}}</h3>
            <!-- RELOAD GAME & HELP -->
            <div class="buttons">
                <span><i class="fa-solid fa-arrows-rotate fa-2xl"></i></span>
                <span><i class="fa-sharp fa-solid fa-question fa-2xl"></i></span>
            </div>
        </header>

        <!-- GAME -->
        <div class="game">
            <div class="rows">
                <div v-for="(row, index) in rowletters" :key="index">
                    <Row :rowletter="row"/>
                </div>
            </div>
            

            <div class="keyword">
                <Keyboard 
                    @keywordletter = "listenerKeywordScreen"
                />
            </div>
        </div>
     </div>
</template>

<script lang="ts">
    import { Vue, Options } from 'vue-class-component';
    import { Prop } from 'vue-property-decorator';
    import Keyboard from './Keyboard.vue';
    import Row from './Row.vue'

    /************* TYPES **************/
    import { RowLetter } from '../types/types'
    /************* FINAL **************/

    @Options({
        components: {
            Row,
            Keyboard
        }
    })
    export default class GameLanding extends Vue {
        
        @Prop() readonly rowletters!: RowLetter[]
        @Prop() readonly title!: String
        
        created() {

        }

        listenerKeywordScreen(letter: string): void {
            this.$emit('keywordletter', letter)
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

    .rows {
        display: flex;
        flex-direction: column;
        gap: 0.5em;
    }

    .links {
        cursor: pointer;
    }
</style>