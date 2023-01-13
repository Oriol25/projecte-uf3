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
                if (false) { // TODO: SI HAGO UNA COMBINACION DE LETRAS COMO CTRL + R, CTRL + C etc. NEGAR -- COMO SE HACE ESO IDK
                   return; 
                }

                this.pushLetter(event.key)

                return;
            }

            if(event.code == 'Backspace') {
                if (false) { // NO HAY UNA LETRA EN EL ROW?
                    return;
                }
                // TODO: BORRAR ULTIMA LETRA
                return;
            }

            if (event.code == 'Enter') {
                if (false) { // TODO: EL ROW ES INCOMPLETO?
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
    }
    /************* FINAL **************/
</script>

<style>


</style>
