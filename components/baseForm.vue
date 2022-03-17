<template>
<v-row>
  <v-col cols="12">
    <h3>{{ user.displayName || user.uid }} さん</h3>
    <h3>ストレスチェック</h3>
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
