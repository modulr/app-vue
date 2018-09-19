<template>
  <v-content>
    <v-container fluid fill-height>
      <v-layout align-center justify-center>
        <v-flex xs12 sm8 md4>
          <v-card class="elevation-1">
            <v-toolbar dark color="primary" class="elevation-0">
              <v-toolbar-title>Login</v-toolbar-title>
              <v-spacer></v-spacer>
            </v-toolbar>
            <v-card-text>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-text-field prepend-icon="person" label="E-mail" type="email"
                  v-model="email"
                  :rules="emailRules"
                  :error-messages="emailErrors"
                  required
                  ></v-text-field>
                <v-text-field prepend-icon="lock" label="Password" type="password"
                  v-model="password"
                  :rules="passwordRules"
                  required
                  ></v-text-field>
              </v-form>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" @click="login">Login</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
    <v-snackbar
      v-model="snackbar"
      :bottom="true"
    >
      {{ text }}
      <v-btn
        color="pink"
        flat
        @click="snackbar = false"
      >
      Close
      </v-btn>
    </v-snackbar>
  </v-content>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      valid: true,
      snackbar: false,
      text: null,
      email: null,
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid'
      ],
      emailErrors: [],
      password: null,
      passwordRules: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 5) || 'Password must be more than 5 characters'
      ],
      passwordErrors: []
    }
  },
  methods: {
    login () {
      if (this.$refs.form.validate()) {

        let self = this

        this.emailErrors = []
        this.passwordErrors = []

        let data = {
          email: this.email,
          password: this.password
        };

        axios.post('http://api-laravel-passport.test/api/auth/login', data)
        .then((response) => {
          console.log(response.data)
        })
        .catch(function (error) {
          if (error.response.status == 422) {
            if (error.response.data.errors.email) {
              self.emailErrors = error.response.data.errors.email
            }
            if (error.response.data.errors.password) {
              self.passwordErrors = error.response.data.errors.password
            }
          }
          if (error.response.status == 401) {
            self.text = error.response.statusText
            self.snackbar = true
          }
        })

      }
    }
  }
}
</script>
