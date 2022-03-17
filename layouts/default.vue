<template>
  <v-app>
    <v-app-bar fixed app>
      <v-toolbar-title>
        <nuxt-link to="/" class="text-decoration-none font-weight-medium">
          Stress Check
        </nuxt-link>
      </v-toolbar-title>
      <v-spacer />
      <v-toolbar-items>
        <v-tooltip bottom>
          <template #activator="{ on, attrs }">
            <v-btn
              v-if="$store.getters.isAuthenticated"
              large
              icon
              color="primary"
              dark
              v-bind="attrs"
              v-on="on"
              @click="logout"
            >
              <v-icon>mdi-logout</v-icon>
            </v-btn>
            <v-btn
              v-else
              large
              icon
              color="primary"
              dark
              v-bind="attrs"
              to="/login"
              v-on="on"
            >
              <v-icon>mdi-account-circle</v-icon>
            </v-btn>
          </template>
          <span v-if="$store.getters.isAuthenticated">Logout</span>
          <span v-else>Login</span>
        </v-tooltip>
        <v-tooltip bottom>
          <template #activator="{ on, attrs }">
            <v-btn
              v-if="!$store.getters.isAuthenticated"
              large
              icon
              color="primary"
              dark
              v-bind="attrs"
              to="/signup"
              v-on="on"
            >
              <v-icon>mdi-account-group-outline</v-icon>
            </v-btn>
          </template>
          <span>Signup</span>
        </v-tooltip>
      </v-toolbar-items>

    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { signOut } from 'firebase/auth'
import { auth } from '~/plugins/firebase'

export default {
  name: 'DefaultLayout',
  data() {
    return {
    }
  },
  methods: {
    logout () {
      signOut(auth).then(() => {
        this.$store.dispatch('setUser', null)
      })
      window.location.href = '/'
    }
  }
}
</script>
