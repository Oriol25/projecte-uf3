<template>
    <div class="content" id="content">
        <!-- NAV -->
        <header class="nav-game">
            <!-- STATISTICS -->
            <a href="#"><i class="fa-solid fa-chart-column fa-2xl"></i></a>
            <!-- TITLE -->
            <h3>WORDLE</h3>
            <!-- RELOAD GAME & HELP -->
            <div class="buttons">
                <a href="#"><i class="fa-solid fa-arrows-rotate fa-2xl"></i></a>
                <a href="#"><i class="fa-sharp fa-solid fa-question fa-2xl"></i></a>
            </div>
        </header>

        <!-- GAME -->
        <div class="game">
            <Row v-for="(rowI, index) in rowWord" :key="index"/>
        </div>

        <!-- KEYWORD -->
        <div>
            
        </div>
     </div>
</template>

<script lang="ts">
    import { defineComponent } from 'vue'
    import Row from './Row.vue'

    export default defineComponent({
        components: {
            Row
        },
        data() {
            return {
                rowWord: [1,2,3,4,5,6]
            }
        }
    })
</script>

<style scoped>
    .game {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 1em;
        gap: 0.5em;
    }
</style>