<template>
    <div class="row">
        <div v-for="(i, index) in arrayLetters" :key="index" @click="getLetter(i)" class="letter-keyboard">
            {{i}}
        </div>
    </div>
    <div class="row">
        <button v-for="(i, index) in arrayLetters2" :key="index" @click="getLetter(i)" class="letter-keyboard" :id="i">
            {{i}}
        </button>
    </div>
</template>

<script>
    import { defineComponent } from 'vue'

    export default defineComponent({
        data() {
            return {
                arrayLetters: ['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'],
                arrayLetters2: ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L', 'Ç']
            }           
        },
        methods: {
            getLetter(letter){
                console.log(letter)
            }
        }
        
    })
</script>

<style>
    .row {
        display: flex;
        flex-direction: row;
        gap: 5px;

    }
    .letter-keyboard {
        border: 1px solid rgb(207, 206, 206);
        background: rgb(227, 227, 227);
        border-radius: 3px;
        margin-top: 5px;

        width: 50px;
        height: 50px;

        font-size: 25px;
        text-align: center;
        line-height: 50px;
    }
</style>