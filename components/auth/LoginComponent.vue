<template>
  <v-container fill-height>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="6">
        <v-form ref="form">
          <v-text-field v-model="form.email" :counter="10" label="Email" color="green" required>
            <v-icon slot="prepend" color="grey">
              mdi-email
            </v-icon>
          </v-text-field>

          <v-text-field v-model="form.password" label="Password" type="password" color="green" required>
            <v-icon slot="prepend" color="grey">
              mdi-key
            </v-icon>
          </v-text-field>

          <v-btn color="blue-grey" class="ml-0" @click="login">
            Login
          </v-btn>
        </v-form>
        <v-btn to="/register">Register</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'Login',
    data() {
      return {
        form: {
          email: '',
          password: ''
        }
      }
    },

    methods: {
      async login() {
        // this is managed automatically in the background
        // await this.$axios.$get('/sanctum/csrf-cookie')
        try {
          let response = await this.$auth.loginWith('laravelSanctum', {
            data: this.form
          })
          // console.log(response)
          this.$router.push('/dashboard')
        } catch (err) {
          console.log(err)
        }
      }
    }
  }

</script>
