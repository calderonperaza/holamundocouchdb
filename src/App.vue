<template>
  <v-app>
    <v-app-bar
      app
      color="secondary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
      <h2>Demo de Couch DB</h2>
      </div>

      <v-spacer></v-spacer>
        
        <v-chip class="mr-2" color="accent"> 
          <v-chip  color="accent"><v-img class="ml-0" src="./assets/yo.png"></v-img></v-chip>         
          calderonperaza@gmail.com
          </v-chip>
        
      
    </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import HelloWorld from './components/HelloWorld.vue';

export default Vue.extend({
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
});
</script>
