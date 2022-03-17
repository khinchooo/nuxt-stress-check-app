<template>
<div>
  <v-row>
  <v-col>
    <h3>{{ $store.state.user.displayName || $store.state.user.uid }} さん</h3>
    <h3>ストレスチェック</h3>
  </v-col>
</v-row>
</div>

</template>

<script>
export default {
  name: 'BaseComponent',
}
</script>
