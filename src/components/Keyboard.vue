<template>
    <div class="row" v-for="(i, index) in arrayLetters" :key="index">
        <button v-for="(k, ind) in i" :key="ind" @click="getLetter(k)" class="letter-keyboard">
            {{k}}
        </button>
    </div>
</template>

<script>
    import { defineComponent } from 'vue'

    export default defineComponent({
        data() {
            return {
                arrayLetters: [['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'],
                               ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L', 'Ç'],
                               ['ENTER', 'Z', 'X', 'C', 'V', 'B', 'N', 'M', '-']],
                    }           
        },
        methods: {
            getLetter(letter){
                this.$emit('keywordletter', letter)
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