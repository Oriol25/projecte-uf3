<template>
    <div>
        <Login v-if="showLogin" @profile="profile = $event"/>
        <GameLanding v-if="showLanding"/>
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
    
    @Options({
        components: {
            GameLanding,
            Login
        }
    })
    export default class GameControl extends Vue {
        profile: Person = {
            name: '',
            email: '',
            tel: '',
        }
        showLogin: boolean = true
        showLanding: boolean = false

        @Watch('profile.name')
        onDataChanged(value: string, oldValue: string) {
            if (this.profile.name && this.profile.email && this.profile.tel) {
                this.showLogin = false
                this.showLanding = true
            }
        }


    }
</script>

<style>


</style>
