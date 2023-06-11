<script>
import GameTitle from './components/GameTitle.vue'
import PlayGame from './components/PlayGame.vue'

export default {
  components: {
    GameTitle,
    PlayGame
  }
}
</script>

<template>
  <header>
    <div class="wrapper">
      <game-title />
    </div>
  </header>
  <main>
    <play-game>
      <a href="https://www.flaticon.com/fr/icones-gratuites/tic-tac-toe" title="tic tac toe icônes">
        Tic tac toe icônes créées par Freepik - Flaticon
      </a>
    </play-game>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

a {
  color: rgba(235, 235, 235, 0.64);
}
</style>
