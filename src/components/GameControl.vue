<template>
    <div>
        <Login 
            v-if="showLogin"
            @profile="profile = $event"
        />
        <GameLanding 
            v-if="showLanding"
            :rowletters="rowLetters"
        />
    </div>

</template>

<script lang="ts">
    import { Options, Vue } from 'vue-class-component';
    import { Watch } from 'vue-property-decorator';

    import GameLanding from './GameLanding.vue'
    import Login from './Login.vue'

    type Person = {
        name: String,
        email: String,
        tel: String
    }

    type Letter = {
        letter: String,
        status: String
    }

    type Row = Letter[]
    
    @Options({
        components: {
            GameLanding,
            Login
        }
    })
    export default class GameControl extends Vue {
        
        showLogin: boolean = false
        showLanding: boolean = true

        profile: Person = {
            name: '',
            email: '',
            tel: '',
        }

        rowLetters: Row[] = []
        

        @Watch('profile.name')
        onDataChanged(value: string, oldValue: string) {
            if (this.profile.name && this.profile.email && this.profile.tel) {
                this.showLogin = false
                this.showLanding = true
            }
        }

        created() {
            this.addRow()
        }

        addRow() {
            let row: Letter = {
                letter: '',
                status: 'secondary'
            }

            let rowLetter: Letter[] = []
            for (var i = 0; i < 5; i++) {
                rowLetter.push(row)
            }

            this.rowLetters.push(rowLetter)
        }

    }
</script>

<style>


</style>
