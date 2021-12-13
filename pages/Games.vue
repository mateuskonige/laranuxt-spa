<template>
  <v-container fill-height>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="6">
        <games-component />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import GamesComponent from '../components/GamesComponent.vue'
export default {
    components: { GamesComponent },

}
</script>

<style>

</style>