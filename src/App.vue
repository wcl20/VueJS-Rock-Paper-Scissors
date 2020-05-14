<template>
  <div id="app">
    <div id="score">
    <h2>Player</h2>
    <h1>{{ score.player }} : {{ score.computer }}</h1>
    <h2>AI</h2>
    </div>
    <div id="output">
      <img :src="require('@/assets/' + move.player + '.png')"/>
      <img :src="require('@/assets/' + move.computer + '.png')"/>
    </div>
    <div>
      <Move :move="moves.ROCK" v-on:click.native="play(moves.ROCK)"/>
      <Move :move="moves.PAPER" v-on:click.native="play(moves.PAPER)"/>
      <Move :move="moves.SCISSORS" v-on:click.native="play(moves.SCISSORS)"/>
      <div>Select your move</div>
    </div>
  </div>
</template>

<script>
import Move from "@/components/Move.vue"

// Possible moves
const moves = {
  ROCK: "rock",
  PAPER: "paper",
  SCISSORS: "scissors"
};

// Possible results
const results = {
  COMPUTER: "computer",
  PLAYER: "player",
  DRAW: "draw"
}

export default {
  name: 'App',

  components: {
    Move
  },

  data() {
    return {
      moves,
      results,
      score: { player: 0, computer: 0 },
      move: { player: moves.ROCK, computer: moves.ROCK },
    }
  },

  methods: {
    play: function(playerMove) {
      // Make player move
      this.move.player = playerMove;
      // Computer computer move
      let computerMove = this.strategy(playerMove);
      this.move.computer = computerMove;
      // Get results
      let result = this.winner(playerMove, computerMove);
      // Increment score
      if (result == results.COMPUTER) {
        this.score.computer += 1;
      } else if (result == results.PLAYER) {
        this.score.player += 1;
      }
    },

    // Computer strategy
    strategy: function(move) {
      // Let player win in first few rounds
      if (this.score.player < 5) {
        let choice = Math.floor(Math.random() * Object.keys(moves).length);
        return moves[Object.keys(moves)[choice]];
      }
      // Destroy player
      switch (move) {
        case moves.ROCK: return moves.PAPER
        case moves.PAPER: return moves.SCISSORS
        case moves.SCISSORS: return moves.ROCK
      }
    },

    // Get Winner
    winner: function(playerMove, computerMove) {
      let player = Object.keys(moves).indexOf(playerMove.toUpperCase());
      let computer = Object.keys(moves).indexOf(computerMove.toUpperCase());
      // Return result
      if ((player + 1) % 3 == computer) return results.COMPUTER;
      if (player == computer) return results.DRAW;
      return results.PLAYER;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}

#score {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

#output {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

img {
  height: 50vh;
}
</style>
