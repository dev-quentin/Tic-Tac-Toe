<script>
import GameGrid from './GameGrid.vue'
import CrossIcon from './icons/CrossIcon.vue'
import CircleIcon from './icons/CircleIcon.vue'

export default {
  components: {
    GameGrid,
    CrossIcon,
    CircleIcon
  },
  data() {
    return {
      noPlayer: 0,
      crossPlayer: 1,
      criclePlayer: 2,
      currentPlayer: 0,
      players: {
        1: [],
        2: []
      },
      countCellPlayed: 0,
      winningConditions: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ]
    }
  },
  computed: {
    winner() {
      let winner = false

      for (let player in this.players) {
        if (this.mathWinningCondition(player)) {
          winner = parseInt(player)
        }
      }

      if (winner === false && (this.countCellPlayed < 0 || this.countCellPlayed >= 9)) {
        winner = this.noPlayer
      }

      return winner
    }
  },
  methods: {
    mathWinningCondition(player) {
      let result = false
      for (let winningCondition of this.winningConditions) {
        const cellNotPlayed = winningCondition.filter(
          (cellIndex) => !this.players[player].includes(cellIndex)
        )
        if (cellNotPlayed && cellNotPlayed.length > 0) {
          continue
        }

        result = true
        break
      }

      return result
    },
    cellPlayed(cell) {
      this.players[this.currentPlayer].push(cell)
      this.countCellPlayed++
      this.nextPlayer()
    },
    nextPlayer() {
      if (this.currentPlayer == this.crossPlayer) {
        this.currentPlayer = this.criclePlayer
      } else {
        this.currentPlayer = this.crossPlayer
      }
    },
    play() {
      this.currentPlayer = this.crossPlayer
    },
    replay() {
      this.currentPlayer = this.noPlayer
      this.players[this.crossPlayer] = []
      this.players[this.criclePlayer] = []
      this.countCellPlayed = 0
      this.currentPlayer = this.crossPlayer
    }
  }
}
</script>

<template>
  <section>
    <header class="title">
      <p>
        <template v-if="winner === false">
          <span v-show="currentPlayer !== noPlayer">Au tour de :</span>
          <div class="icon-container">
            <KeepAlive>
              <cross-icon class="icon" v-if="crossPlayer === currentPlayer" />
              <circle-icon class="icon" v-else-if="criclePlayer === currentPlayer" />
              <button class="button" v-else @click="play">Play</button>
            </KeepAlive>
          </div>
          <slot v-if="currentPlayer === noPlayer"></slot>
        </template>
        <template v-else>
          <span v-if="winner !== 0">Partie gagner par :</span>
          <span v-else>Match nul</span>
          <div class="icon-container">
            <KeepAlive>
              <cross-icon class="icon" v-if="crossPlayer === winner" />
              <circle-icon class="icon" v-else-if="criclePlayer === winner" />
            </KeepAlive>
          </div>
          <button class="button" @click="replay">Rejouer</button>
        </template>
      </p>
    </header>
    <main>
      <game-grid
        v-show="currentPlayer !== noPlayer"
        :finished="winner !== false"
        :players="players"
        :cross-player="crossPlayer"
        :cricle-player="criclePlayer"
        @cellPlayed="cellPlayed($event)"
      />
    </main>
  </section>
</template>

<style scoped>
p {
  text-align: center;
}

.icon-container {
  display: flex;
  justify-content: center;
}

.icon {
  height: 90px;
  width: 90px;
}

.button {
  background-color: rgba(235, 235, 235, 0.64);
  box-shadow: 3px 4px 7px #5a5a5a;
  border: none;
  border-radius: 10px;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}
</style>
