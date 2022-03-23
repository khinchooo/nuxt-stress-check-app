<template>
  <v-row class="text-center">
    <v-col>
      <h3>{{ user.displayName || user.uid }} さん</h3>
      <span>ストレスチェック</span>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'BaseFormComponent',
  computed: {
    user () {
      return this.$store.state.user
    }
  }
}
</script>
