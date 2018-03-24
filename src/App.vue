<template>
  <div :style="{
    display: 'flex',
    width: '100%',
    'overflow-x': 'hidden',
    flexDirection: 'column',
    justifyContent: 'center',
    alignItems: 'center',
    marginBottom: '20px',
  }">
    <Game/>
  </div>
</template>

<script>
import Game from './components/Game/Game'

export default {
  name: 'App',
  components: {
    Game,
  }
}
</script>