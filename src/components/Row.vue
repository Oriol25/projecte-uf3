<template>
    <div class="row no-seleccionable">
        <div v-for="(letter, index) in rowletter" :key="index" :class="['letter', letter.status]">
            {{letter.letter.toUpperCase()}}
        </div>
    </div>
</template>

<script lang="ts">
    import { Vue } from 'vue-class-component';
    import { Prop } from 'vue-property-decorator';

    type Letter = {
        letter: String,
        status: String
    }

    type RowLetter = Letter[]

    export default class Row extends Vue {
        
        @Prop() readonly rowletter!: RowLetter
        
    }

</script>

<style scoped>
    .row {
        display: flex;
        flex-direction: row;
        gap: var(--separacionLetter);

    }
    .letter {

        border: 1px solid #333;
        border-radius: 8px;

        width: 50px;
        height: 50px;

        font-size: 35px;
        text-align: center;
    }

    .secondary {
        background: #eee;
    }

    .warning {
        background: yellow;
    }

    .success {
        background: green;
    }

    .no-seleccionable {
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none; 
    }

</style>