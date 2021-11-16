<template>
  <section class="tic-tac-toe-game">
    <div class="board" :class="{'board--active': !isGameWon}">
      <div id="0" class="board__field" @click="tileClicked"></div>
      <div id="1" class="board__field" @click="tileClicked"></div>
      <div id="2" class="board__field" @click="tileClicked"></div>
      <div id="3" class="board__field" @click="tileClicked"></div>
      <div id="4" class="board__field" @click="tileClicked"></div>
      <div id="5" class="board__field" @click="tileClicked"></div>
      <div id="6" class="board__field" @click="tileClicked"></div>
      <div id="7" class="board__field" @click="tileClicked"></div>
      <div id="8" class="board__field" @click="tileClicked"></div>
    </div>

    <section class="information">
      <p v-if="!isGameWon">Current player: {{ currentPlayer }}</p>
      <p v-if="!isGameWon">Turns left: {{ turnsLeft }} </p>

      <h3 v-if="winner !== ''">
        <span v-if="winner !== 'TIE'">
          <span v-if="winner === 'PLAYERX_WON'">Player X</span>
          <span v-if="winner === 'PLAYERO_WON'">Player O</span>
          is the winner!
        </span>
        <span v-else>
          It's a tie!
        </span>
      </h3>
    </section>
    <section class="action">
      <button @click="resetGame" class="btn">Reset Game</button>
    </section>
  </section>
</template>

<script>
const PLAYERX_WON = 'PLAYERX_WON';
const PLAYERO_WON = 'PLAYERO_WON';
const TIE = 'TIE';
const WIN_CONDITIONS = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
];

export default {
  name: 'GameBoard',
  data() {
    return {
      currentPlayer: 'X',
      board: ['', '', '', '', '', '', '', '', ''],
      isGameActive: 'true',
      winner: '',
      totalRounds: 9,
      roundsPlayed: 0,
    }
  },
  methods: {
    tileClicked(event) {
      if (!this.isGameActive)
        return

      if (event && this.validAction(event.target)) {
        this.roundsPlayed++;
        this.setTileContent(event.target);
        this.updateBoard(event.target)
        this.updateCurrenPlayer();
        this.evaluateGame();
      } else {
        window.alert('not a valid action');
      }
    },
    validAction(tile) {
      if (tile.classList.contains('X') || tile.classList.contains('O')) {
        return false;
      }
      return true;
    },
    setTileContent(tile) {
      tile.classList.add(this.currentPlayer);
    },
    updateBoard(tile) {
      this.board[tile.id] = this.currentPlayer;
    },
    updateCurrenPlayer() {
      this.currentPlayer === 'X' ? this.currentPlayer = 'O' : this.currentPlayer = 'X';
    },
    evaluateGame() {
      if (!this.isGameActive) {
        return;
      }

      WIN_CONDITIONS.forEach((condition) => {
        let result = condition.filter((id) => this.board[id] === 'X');
        if (result.length === 3) {
          this.isGameActive = false;
          this.winner = PLAYERX_WON;
        }

        result = condition.filter((id) => this.board[id] === 'O');
        if (result.length === 3) {
          this.isGameActive = false;
          this.winner = PLAYERO_WON;
        }
      });

      if (this.roundsPlayed === 9) {
        this.isGameActive = false;
        this.winner = TIE;  
      }
    },
    resetGame() {
      this.currentPlayer = 'X';
      this.board = ['', '', '', '', '', '', '', '', ''];
      this.isGameActive = 'true';
      this.winner = '';
      this.totalRounds = 9;
      this.roundsPlayed = 0;

      document.querySelectorAll('.board__field').forEach(item => item.classList.remove('X', 'O'))
    }
  },
  computed: {
    isGameWon() {
      return this.winner !== '';
    },
    turnsLeft() {
      return this.totalRounds - this.roundsPlayed;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .board {
    display: grid;
    grid-template-columns: repeat(3, calc(100% / 3));
    grid-template-rows: repeat(3, 100px);
    border: 3px solid var(--text-color);
  }

  .board--active .board__field {
    background: var(--bg-color);
    color: var(--text-color);
  }

  .board--active .board__field:hover {
    background: var(--bg-color-hover);
  }

  .board__field {
    background: var(--bg-color-inactive);
    border: 3px solid var(--text-color);
    color: var(--text-color-inactive);
    width: 100%;
    height: 100%;
    position: relative;
    cursor: pointer;
    transition: all .3s ease-in-out;
  }

  .board__field:hover {
    background: var(--bg-color-inactive);
  }

  .board__field.X::before,
  .board__field.O::before {
    position: absolute;
    content: 'X';
    font-size: 40px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .board__field.O::before {
    content: 'O';
  }

  .btn {
    border: 1px solid var(--text-color);
    border-radius: 3px;
    font-size: 1rem;
    color: var(--text-color);
    background-color: var(--bg-color);
    padding: 5px 15px;
    transition: all .3s ease-in-out;
    cursor: pointer;
  }

  .btn:hover {
    background-color: var(--text-color);
    color: var(--bg-color);
  }
</style>
