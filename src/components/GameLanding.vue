<template>
    <div class="content" id="content">
        <!-- NAV -->
        <header class="nav-game">
            <!-- STATISTICS -->
            <span @click="showStats"><i class="fa-solid fa-chart-column fa-2xl"></i></span>
            <!-- TITLE -->
            <h3>{{title}}</h3>
            <!-- RELOAD GAME & HELP -->
            <div class="buttons">
                <span @click="newGame"><i class="fa-solid fa-arrows-rotate fa-2xl"></i></span>
                <span @click="showInformation"><i class="fa-sharp fa-solid fa-question fa-2xl"></i></span>
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
    import Swal from 'sweetalert2'

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
        @Prop() readonly winned_games_counter!: Number
        @Prop() readonly tryies_better_game!: Number
        @Prop() readonly time_better_game!: Number
        
        game_counter : number = 1

        created() {

        }

        listenerKeywordScreen(letter: string): void {
            this.$emit('keywordletter', letter)
        }

        showInformation(): void {
            Swal.fire({
                icon: 'info',
                title: '¿Como jugar al WordleIBC?',
                html: '<img src="./img/info_ok.jpg"/>',
            })
        }

        newGame(): void {
            this.game_counter++
            this.$emit('newGame', true)
        }

        showStats() : void {
            Swal.fire({
                icon: 'info',
                title: 'Estadísticas',
                html: `Nombre del jugador: <br>
                       Partidas realizadas: ${this.game_counter} <br>
                       Partidas ganadas: ${this.winned_games_counter} <br>
                       Mejor partida: ${this.tryies_better_game} <br>
                       Partida mas rapida: ${this.time_better_game}`,
            })
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