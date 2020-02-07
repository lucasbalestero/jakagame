<template>
  <v-app style="background-color: green">
    <v-toolbar app dark>
      <v-toolbar-title class="headline text-uppercase">
        <div id="logo" @click="resetar()">
          <span style="font-weight: bold">JAKA</span>
          <span class="font-weight-light"> Games</span>
        </div>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn flat href="https://github.com/vuetifyjs/vuetify/releases/latest" target="_blank">
        <span class="mr-2">Cadastrar</span>
      </v-btn>
    </v-toolbar>

    <v-content>
      <GameCardField/>
    </v-content>

    <v-footer dark>
      <v-spacer></v-spacer>
      <div class="mr-2">JAKA Games &copy; {{ new Date().getFullYear() }}</div>
    </v-footer>

  </v-app>
</template>

<script>
import GameCardField from './components/GameCardField'

export default {
  name: 'App',
  
  components: {
    GameCardField,
  },
  
  data: () => ({
  
  }),

  methods: {
    resetar(){
      window.location.reload()
    }
  }

}
</script>
<style>
  #logo:hover {
    cursor: pointer;
  }
</style>