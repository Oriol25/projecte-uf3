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
            @keywordletter = "listenerKeywordScreen"
        />
    </div>

</template>

<script lang="ts">
    /************* LLIBRERIES **************/
    import { Options, Vue } from 'vue-class-component'; // CONVIERTE LOS COMPONENTES EN CLASES
    import { Watch } from 'vue-property-decorator'; // ARREGLO PARA AÑADIR LAS OPCIONES DE VUE DENTRO DE LAS CLASES 
    import $ from 'jquery'; // JQUERY
    import { dic } from '../assets/js/diccionari';
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
        misteryWord: String = 'BARRO'

        showLogin: boolean = false
        showLanding: boolean = true

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
            }
        }

        created(): void {
            this.addRow()
            $(document).on("keyup", this.listenerKeyword)
            // this.palabra()
            console.log(this.misteryWord)
        }

        palabra(): void {
            let randomNumber =  Math.floor(Math.random() * this.diccionari.length);
            this.misteryWord = this.diccionari[randomNumber].toUpperCase()
        }

        listenerKeywordScreen(letter: string): void {
            if(letter != 'ENTER' && letter != '-'){
                this.pushLetter(letter)
            }
            
            if (letter == 'ENTER') {
                if (this.rowLetters[this.rowLetters.length - 1][4].letter == '') {
                    return;
                }

                if (this.rowLetters.length < 5) {
                    let splitMisteryWord = this.misteryWord.split("")
                    let splitWord: any[] = []; // TODO: TYPE
                    
                    splitMisteryWord.forEach((element, index) => {
                        splitWord.push(this.rowLetters[this.rowLetters.length - 1][index].letter)
                    })
                    
                    let perfectMatch : any[] = []; // aquí guardamos los elementos exactos
                    let almostMatch : any[] = []; 

                    splitWord.forEach((elemento, indice) => {
                        if (elemento == splitMisteryWord[indice]) {
                            perfectMatch.push(elemento);
                            this.rowLetters[this.rowLetters.length - 1][indice].status = "success"  // existe en esa misma posición
                        } else if (splitMisteryWord.indexOf(elemento) > -1) {
                            almostMatch.push(elemento);  // existe pero en otra posición
                            this.rowLetters[this.rowLetters.length - 1][indice].status = "warning"  // existe en esa misma posición
                        }
                    });

                    if (perfectMatch.length == splitWord.length) { // HE GANADO?
                        alert('HAS  GANADO');
                        return;
                    }
                    
                    this.addRow()
                } else {
                    alert('HAS  PERDIDO');
                    return;
                }
            }

            if (letter == '-'){
                if (this.rowLetters[this.rowLetters.length - 1][0].letter == '') {
                    return;
                }

                this.unpushLetter()
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

        listenerKeyword(event: any) : void { // TODO: QUE TYPE UTILIZA EL event, CAMBIAR TYPE ANY -- IDK
            if (false) { // TODO: NO ESTOY JUGANDO?
                return;
            }

            if (event.code.startsWith('Key') || event.code == 'Backslash') {
                this.pushLetter(event.key)

                return;
            }

            if(event.code == 'Backspace') {
                if (this.rowLetters[this.rowLetters.length - 1][0].letter == '') {
                    return;
                }
                this.unpushLetter();

                return;
            }

            if (event.code == 'Enter') {
                if (this.rowLetters[this.rowLetters.length - 1][4].letter == '') {
                    return;
                }

                if (this.rowLetters.length < 5) {
                    this.addRow()
                    
                    if (false) { // HE GANADO?
                        return;
                    }

                    if (false) { // HE PERDIDO?

                    }

                }
            }
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
    }
    /************* FINAL **************/
</script>

<style>


</style>
