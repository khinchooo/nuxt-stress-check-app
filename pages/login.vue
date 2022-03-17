<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <login-form />
    </v-col>
  </v-row>
</template>

<script>
import loginForm from '~/components/loginForm.vue'

export default {
  name: 'LoginPage',
  components: { loginForm }
}
</script>
