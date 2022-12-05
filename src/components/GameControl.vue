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
    import $ from 'jquery' // JQUERY
    import { dic } from '../assets/js/diccionari';
    /************* FINAL **************/


    /************* COMPONENTS EXTERNS **************/
    import GameLanding from './GameLanding.vue'
    import Login from './Login.vue'
    /************* FINAL **************/

    /*
     * https://www.primefaces.org/primevue/setup
     * https://www.primefaces.org/primeflex/
    */

    /************* TYPES **************/
    type Person = {
        name: String,
        email: String,
        tel: String
    }

    type Letter = {
        letter: String,
        status: String
    }

    type RowLetter = Letter[]
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

        profile: Person = {
            name: '',
            email: '',
            tel: '',
        }

        rowLetters: RowLetter[] = []        

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

            let rowLetter: Letter[] = []

            for (var i = 0; i < 5; i++) {
                const row: Letter = {
                    letter: '',
                    status: 'secondary'
                }
                rowLetter.push(row)
            }

            this.rowLetters.push(rowLetter)
        }

        listenerKeyword(event: any) : void { // TODO: QUE TYPE UTILIZA EL event, CAMBIAR TYPE ANY
            if (true) { // TODO: ESTOY JUGANDO?
                if (event.code.startsWith('Key') || event.code == 'Backslash') {
                    if (true) { // TODO: SI HAGO UNA COMBINACION DE LETRAS COMO CTRL + R, CTRL + C etc. NEGAR
                        this.pushLetter(event.key)
                    }
                } else if(event.code == 'Backspace') {
                    // TODO: BORRAR ULTIMA LETRA
                } else if (event.code == 'Enter') {
                    if (true) { // TODO: EL ROW ES COMPLETO?
                        if (true) {  // TODO: COMPROBAR SI LA PALABRA ES CORRECTA, FUNCION QUE DEVUELBA TRUE O FALSE + CAMBIO DE COLORES? NEGAR
                            if (this.rowLetters.length < 5) {
                                this.addRow()
                            } else {
                                // TODO: PIERDO?
                            }
                        } else {
                            // TODO: GANO?
                        }
                    }
                }
            }
            
            
        }

        pushLetter(letter: String): void {

            const rowLetter = this.rowLetters.length-1
            const lastPostionLetter = 4

            let index: number = this.rowLetters[rowLetter].findIndex(function (element: Letter): boolean {
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
