<template>
    <board ref="board"></board>
    <boardForm @change-value="changeValue"></boardForm>
</template>

<script>
import board from './components/board';
import boardForm from './components/form'
export default {
    name: 'App',
    data() {
        return {
            boardData: {
                width: 0,
                height: 0,
            },
        }
    },
    components: {
        board,
        boardForm
    },
    methods: {
        changeValue(data) {
            this.$refs.board.setDataEvent(data);
        },
    },
}
</script>

<style>
html ,body { width: 100%; height: 100%; overflow: hidden; position: absolute; padding: 0; margin: 0;}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
