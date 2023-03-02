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

    /************* TYPES **************/
    import { RowLetter } from '../types/types'
    /************* FINAL **************/

    export default class Row extends Vue {
        
        @Prop() readonly rowletter!: RowLetter
        
    }

</script>

<style scoped>
    .row {
        display: flex;
        flex-direction: row;
        gap: 0.5em;

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
        background: orange;
    }

    .success {
        background: #21b011;
    }

    .default_error {
        background: rgba(95, 95, 95, 0.796);
    }


</style>