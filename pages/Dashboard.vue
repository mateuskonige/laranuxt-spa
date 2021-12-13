<template>
  <v-container fill-height>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="6">
        <dashboard-component />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import DashboardComponent from '../components/DashboardComponent.vue'
  export default {
    name: 'Dashboard',
    components: {
      DashboardComponent
    }
  }
</script>
