<template>
  <register-component />
</template>
<script>
  import RegisterComponent from '../components/auth/RegisterComponent.vue'
  export default {
      layout: "empty",
    auth: false,
    components: {
      RegisterComponent
    }
  }

</script>
