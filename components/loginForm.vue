<template>
  <v-container>
    <v-card width="400px" class="mx-auto mt-5">
      <v-card-title>
        <h1 class="display-1">
          ログイン
        </h1>
      </v-card-title>
      <v-card-text>
        <v-form ref="login_form">
          <v-text-field
            v-model="email"
            type="email"
            :rules="[required]"
            label="Email"
            prepend-icon="mdi-email"
          />
          <v-text-field
            v-model="password"
            :type="showPassword ? 'text' : 'password'"
            label="Password"
            :rules="[required, min_length, max_length]"
            prepend-icon="mdi-lock"
            :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
            @click:append="showPassword=!showPassword"
          />
          <v-card-actions>
          <v-spacer />
          <v-btn type="submit" class="info" @click.prevent="login">
            ログイン
          </v-btn>
          </v-card-actions>
          <div>{{ message }}</div>
        </v-form>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import { signInWithEmailAndPassword } from '@firebase/auth'
import { auth } from '~/plugins/firebase'

export default {
  name: 'LoginPage',
  data () {
    return {
      email: '',
      password: '',
      message: '',
      showPassword: false,
      required: value => !!value || '😱必須です',
      min_length: value => value.length >= 8 || '短すぎです',
      max_length: value => value.length <= 20 || '長すぎです'
    }
  },
  methods: {
    login () {
      if (this.$refs.login_form.validate()) {
        signInWithEmailAndPassword(auth, this.email, this.password)
          .then((userCredential) => {
            const user = userCredential.user
            this.message = 'Login: ' + (user.email)
            this.$store.dispatch('setUser', {
              uid: user.uid,
              displayName: user.displayName
            })
            this.$router.push('/')
          })
          .catch(() => {
            this.message = 'ログインできません'
          })
      } else {
        this.message = '入力エラーがあります'
      }
    }
  }
}
</script>

<style>

</style>
