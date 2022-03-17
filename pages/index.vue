<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <login-form v-if="!$store.getters.isAuthenticated" />
      <div v-else>
        <base-form />
        <!-- <v-btn to="/questions/A01" class="primary">開始</v-btn> -->
        <v-btn to="/start" class="primary">開始</v-btn>
      </div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
}
