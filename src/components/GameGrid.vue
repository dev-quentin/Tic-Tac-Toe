<script>
import CrossIcon from './icons/CrossIcon.vue'
import CircleIcon from './icons/CircleIcon.vue'

export default {
  components: {
    CrossIcon,
    CircleIcon
  },
  props: {
    finished: {
      type: Boolean,
      required: true
    },
    players: {
      type: Object,
      required: true
    },
    crossPlayer: {
      type: Number,
      required: true
    },
    criclePlayer: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      countCell: 9
    }
  },
  emits: {
    cellPlayed: function (cellIndex) {
      return 0 <= cellIndex && cellIndex >= 8
    }
  },
  methods: {
    isPlayed(cell) {
      return (
        this.players[this.crossPlayer].includes(cell) ||
        this.players[this.criclePlayer].includes(cell)
      )
    },
    getPlayer(cell) {
      if (this.players[this.crossPlayer].includes(cell)) {
        return 'cross-icon'
      } else if (this.players[this.criclePlayer].includes(cell)) {
        return 'circle-icon'
      }
      return null
    },
    play(cell) {
      if (this.finished || this.isPlayed(cell)) {
        return false
      }
      this.$emit('cellPlayed', cell)
    }
  }
}
</script>

<template>
  <div class="grid">
    <div
      v-for="(cell, cellIndex) in countCell"
      :key="cellIndex"
      :class="['cell', isPlayed(cellIndex) ? 'played' : 'not-played']"
      role="button"
      :tabindex="cell"
      @click.prevent="play(cellIndex)"
      @keypress.enter="play(cellIndex)"
    >
      <component class="icon" :is="getPlayer(cellIndex)" />
    </div>
  </div>
</template>

<style>
.grid {
  display: grid;
  grid-template-columns: 33% 33% 33%;
  padding: 1%;
}

.cell {
  height: 60px;
  cursor: pointer;
  border: 1.5px solid rgba(235, 235, 235, 0.64);
  text-align: center;
}

.cell:hover,
.cell:focus {
  background-color: rgba(235, 235, 235, 0.1);
}

.cell .icon {
  height: 60px;
}
</style>
