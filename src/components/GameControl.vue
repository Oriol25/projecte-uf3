<template>
    <div>
        <Login v-if="showLogin" @profile="profile = $event"></Login>
        <GameLanding v-if="showLanding"></GameLanding>
    </div>

</template>

<script lang="ts">
    import { defineComponent } from 'vue'
    import GameLanding from './GameLanding.vue'
    import Login from './Login.vue'

    export default defineComponent({
        watch: {
            profile: {
                handler(val, oldVal){
                    if (val?.name) {
                        this.showLanding = !this.showLanding
                        this.showLogin = !this.showLogin
                        console.log(val)
                    }
                },
                deep: true
            }
        },
        components: {
            GameLanding,
            Login
        },
        data() {
            return {
                profile: undefined,
                showLanding: false,
                showLogin: true,
            }
        },

        methods: {
            
        }
      
    })

</script>

<style>


</style>
