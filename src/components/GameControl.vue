<template>
    <div>
        <Login 
            v-if="showLogin"
            @profile="profile = $event"
        />
        <GameLanding 
            v-if="showLanding"
            :rowletters="rowLetters"
            :title="title"
            :winned_games_counter="winned_games_counter"
            :tryies_better_game="tryies_better_game"
            :time_better_game="time_better_game"
            @keywordletter = "listenerKeyword"
            @newGame = "newGame"
        />
    </div>

</template>

<script lang="ts">
    /************* LLIBRERIES **************/
    import { Options, Vue } from 'vue-class-component'; // CONVIERTE LOS COMPONENTES EN CLASES
    import { Watch } from 'vue-property-decorator'; // ARREGLO PARA AÑADIR LAS OPCIONES DE VUE DENTRO DE LAS CLASES 
    import $ from 'jquery'; // JQUERY
    import { dic } from '../assets/js/diccionari';
    import Swal from 'sweetalert2'
    /************* FINAL **************/


    /************* COMPONENTS EXTERNS **************/
    import GameLanding from './GameLanding.vue'
    import Login from './Login.vue'
    /************* FINAL **************/

    /*
     * https://www.primefaces.org/  /setup
     * https://www.primefaces.org/primeflex/
    */

    /************* TYPES **************/
    import * as customType from '../types/types'
    /************* FINAL **************/
    
    /************* COMPONENT GAME CONTROL **************/
    @Options({
        components: {
            GameLanding,
            Login
        }, 
    })
    export default class GameControl extends Vue {
        
        title: String = 'WORDLE'
        diccionari: String[] = dic
        misteryWord: String = ''
        contador: number = 0
        time_counter: number = 0
        timeout: number = 0
        timer_on: boolean = false

        // GAME STATS
        winned_games_counter : number = 0
        tryies_better_game : number = 0
        time_better_game : number = 0

        showLogin: boolean = true
        showLanding: boolean = false

        ingame: boolean = true

        profile: customType.Person = {
            name: '',
            email: '',
            tel: '',
        }

        rowLetters: customType.RowLetter[] = []        

        @Watch('profile.name')
        onDataChanged(value: string, oldValue: string): void {
            if (this.profile.name && this.profile.email && this.profile.tel) {
                this.showLogin = false
                this.showLanding = true
                this.newGame();
            }   

        
        }

        newGame(): void {
            this.rowLetters = [];
            this.palabra()
            this.addRow()
            this.contador = 1;
            this.time_counter = 0
            this.timeout = 0
            this.startCount()
            
            this.ingame = true
        }

        created(): void {
            $(document).on("keyup", this.listenerKeyword);
        }

        palabra(): void {
            let randomNumber =  Math.floor(Math.random() * this.diccionari.length);
            this.misteryWord = this.diccionari[randomNumber].toUpperCase()
            console.log(this.misteryWord)
        }

        listenerKeyword({code, key}: customType.LetterPress): void {
            if(this.ingame){
                if (false) { // DURANTE LOS SWAL NO PODER ESCRIBIR
                    return;
                }

                if(code != 'Enter' && code != 'Backspace' && code.startsWith('Key')){
                    this.pushLetter(key.toUpperCase())
                }
                
                if (code == 'Enter') {
                    if (this.rowLetters[this.rowLetters.length - 1][4].letter == '') {
                        Swal.fire(
                            'Tienes que completar la palabra',
                            'No puedes dejar celdas vacías, porfavor acaba de escribir la palabra',
                            'error'
                        )
                        return;
                    }

                    if (this.rowLetters.length < 7) {
                        let splitMisteryWord = this.misteryWord.split("")
                        let splitWord: any[] = []; // TODO: TYPE
                        
                        splitMisteryWord.forEach((element, index) => {
                            splitWord.push(this.rowLetters[this.rowLetters.length - 1][index].letter)
                        })
                        
                        if(this.diccionari.indexOf(splitWord.join("").toLowerCase()) == -1) {
                            Swal.fire(
                                'Esta palabra no existe en el diccionario',
                                '',
                                'error'
                            )

                            return;
                        }

                        let perfectMatch : any[] = []; // aquí guardamos los elementos exactos
                        let almostMatch : any[] = []; 

                        splitWord.forEach((elemento, indice) => {
                            if (elemento == splitMisteryWord[indice]) {
                                perfectMatch.push(elemento);
                                this.rowLetters[this.rowLetters.length - 1][indice].status = "success"  // existe en esa misma posición
                            } else if (splitMisteryWord.indexOf(elemento) > -1) {
                                almostMatch.push(elemento);  // existe pero en otra posición
                                this.rowLetters[this.rowLetters.length - 1][indice].status = "warning"  // existe en esa misma posición
                            } else {
                                this.rowLetters[this.rowLetters.length - 1][indice].status = "default_error"
                            }
                        });

                        if (perfectMatch.length == splitWord.length) { // HE GANADO?
                            this.stopCount()
                            Swal.fire(
                                'Enhorabuena! Has ganado!',
                                `Lo has conseguido en ${this.contador} intentos  <br> y en ${this.time_counter} segundos`,
                                'success'
                            )
                            this.winned_games_counter++
                            if(this.tryies_better_game <= this.contador){
                                this.tryies_better_game = this.contador
                            }

                            if(this.time_better_game <= this.time_counter){
                                this.time_better_game = this.time_counter
                            }

                            this.ingame = false
                            return;
                        }
                        this.contador++
                        if(this.rowLetters.length < 6){
                            this.addRow()
                        } else {
                            Swal.fire(
                                    'Ups! Has perdido!',
                                    `Vuelve a intentarlo`,
                                    'error'
                                )
                            this.ingame = false
                            return;
                        }
                    }
                }

                if (code == 'Backspace'){
                    if (this.rowLetters[this.rowLetters.length - 1][0].letter == '') {
                        return;
                    }

                    this.unpushLetter()
                }
            }
        }

        addRow(): void {

            let rowLetter: customType.Letter[] = []

            for (var i = 0; i < 5; i++) {
                const row: customType.Letter = {
                    letter: '',
                    status: 'secondary'
                }
                rowLetter.push(row)
            }

            this.rowLetters.push(rowLetter)
        }

        pushLetter(letter: String): void {

            const rowLetter = this.rowLetters.length-1
            const lastPostionLetter = 4

            let index: number = this.rowLetters[rowLetter].findIndex(function (element: customType.Letter): boolean {
                return element.letter == ''
            })

            if (index === -1) {
                this.rowLetters[rowLetter][lastPostionLetter].letter = letter
                return
            }

            this.rowLetters[rowLetter][index].letter = letter

        }

        unpushLetter(): void {
            const rowLetter = this.rowLetters.length-1
            const lastPostionLetter = 4

            let index: number = this.rowLetters[rowLetter].findIndex(function (element: customType.Letter): boolean {
                return element.letter == ''
            })

            if (index === -1) {
                this.rowLetters[rowLetter][lastPostionLetter].letter = ''
                return
            }

            this.rowLetters[rowLetter][index-1].letter = ''
        }

        timedCount() {
            this.time_counter++;
            this.timeout = setTimeout(this.timedCount, 1000);
        }

        startCount() {
            if (!this.timer_on) {
                this.timer_on = true;
                this.timedCount();
            }
        }

        stopCount() {
            clearTimeout(this.timeout);
            this.timer_on = false;
        }
    }
    /************* FINAL **************/
</script>

<style>


</style>
