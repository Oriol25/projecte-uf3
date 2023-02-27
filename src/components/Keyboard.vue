<template>
    <div class="row" v-for="(i, index) in keyword" :key="index">
        <button v-for="(k, ind) in i" :key="ind" @click="getLetter(k)" class="letter-keyboard">
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
</style>