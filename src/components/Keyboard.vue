<template>
    <div class="row" v-for="(i, index) in keyword" :key="index">
        <button 
            v-for="(k, ind) in i" 
            :key="ind" 
            @click="getLetter(k)" 
            class="letter-keyboard"
            :class="k.code == 'Enter' ? 'enter-button' : ''"
        >
            {{k.key}}
        </button>
    </div>
</template>

<script lang="ts">

import { Vue } from 'vue-class-component'; // CONVIERTE LOS COMPONENTES EN CLASES

import { arrayLetters } from '../assets/js/keyword';
import { LetterPress } from '../types/types';

export default class GameControl extends Vue {

    keyword: LetterPress[][] = arrayLetters;

    getLetter(letter: LetterPress) {
        this.$emit("keywordletter", letter);
    }

}
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

    .enter-button {
        width: 105px;
    }

    @media only screen and (max-width: 600px) {
        .letter-keyboard {
            width: 40px;
            height: 40px;
            font-size: 15px;
            line-height: 40px;
        }

        .enter-button {
            width: 85px;
        }
    }

    @media only screen and (max-width: 550px) {
        .letter-keyboard {
            width: 30px;
            height: 30px;
            font-size: 15px;
            line-height: 30px;
        }

        .enter-button {
            width: 65px;
        }
    }
</style>