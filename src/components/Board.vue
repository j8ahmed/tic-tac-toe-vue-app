<script setup>
    import Square from './Square.vue'
    import { ref } from 'vue';

    const winner = ref(null);
    const player = ref("X");
    const squares = ref(Array(9).fill(null));        // Perhaps I should use the `reactive` function from vue to construct the array state?

    function newGame(){
        squares.value = Array(9).fill(null);
        player.value = "X";
        winner.value = '';
    };

    function nextPlayer(){
        player.value = player.value === "X" ? "O" : "X";
    }

    function makeMove(idx){
        // Allow clicks if the game has yet to be won
        if(!winner.value){
            // Update square if it has not been picked yet
            if(!squares.value[idx]) {
                squares.value.splice(idx, 1, player.value);
                nextPlayer();
            }
            // Check to see if the current player won
            const result = calculateWinner();
            winner.value = result ? result : '';
        }
    }

    function calculateWinner(){
        const lines = [
            [0,1,2],
            [3,4,5],
            [6,7,8],
            [0,3,6],
            [1,4,7],
            [2,5,8],
            [0,4,8],
            [2,4,6],
        ];

        for(let i=0; i < lines.length; ++i){
            const [a,b,c] = lines[i];
            if(
                squares.value[a] &&
                squares.value[a] == squares.value[b] &&
                squares.value[b] == squares.value[c]
            ){
                return squares.value[a];
            }
        }
        return null;
    }

</script>

<template>
    <h1>Tic Tac Toe App</h1>
    <div id="board-header">
        <h2 v-if="winner"> Player {{ winner }} won the game! </h2>
    </div>
    <button id="reset-btn" @click="newGame()">Start New Game</button>
    <div id="squares-container">
        <Square 
            v-for="(square, index) in squares"
            :value="square"
            :index="index"
            :move="makeMove"
        />
    </div>
</template>

<style lang="scss">
    *{
        box-sizing: border-box;
    }
    #reset-btn{
        width: 100%;
        height: 50px;
    }
    #squares-container{
        display: grid;
        grid-template-columns: repeat(3, 100px);
        grid-template-rows: repeat(3, 100px);
        gap: 0px;
    }
    button,h1,h2{
        margin: 16px 0;
        text-align: center;
    }
    button{
        font-size: 24px;
        &:hover{
            cursor: pointer;
        }
    }
</style>

