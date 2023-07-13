<template>
  <v-app>
    <Header
      @generated-rng-key="AssignGeneratedRngKey($event)"
      @settings-config="SetSettings($event)"
      ref="displayedKey"
    />
    <Options/>
    <Board
      :Chosen="wordChosen"
      :settingsConfig="settingsConfig"
      @selected="$refs.displayedKey.chooseRandom()"
    />
  </v-app>
</template>

<script>
import Header from './components/Header.vue'
import Board from './components/Board.vue'

export default {
  name: 'App',
  components: {
    Header,
    Board
  },
  methods: {
    AssignGeneratedRngKey(key) {
      this.wordChosen = key;
    },
    SetSettings(settingsConfig) {
      this.settingsConfig = settingsConfig
      console.log(settingsConfig)
    },
  },
  data () {
    return {
      wordChosen: '',
      settingsConfig: {},
      renderComponent: true,
    }
  }
}
</script>

<style lang="scss">

:root {
  --baseSize: 4em;
}

#app {
  .v-application {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    background-color: #102820;
  }
}


</style>
