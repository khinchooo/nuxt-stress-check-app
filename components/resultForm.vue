<template>
  <v-card color="#FCE4EC" height="400">
    <v-row class="mt-1" align="center" justify="center">
      <v-col cols="12">
        <v-card-text class="text-h6">{{ result.checkDate ? $dateFns.format(result.checkDate.toDate(), 'yyyy/MM/dd HH:mm') : '' }}</v-card-text>
        <v-card-text class="text-h6">判定結果: {{result.hightStress}}</v-card-text>
        <v-card-text class="text">A: {{result.totalA}}点</v-card-text>
        <v-card-text class="text">B: {{result.totalB}}点</v-card-text>
        <v-card-text class="text">C: {{result.totalC}}点</v-card-text>
        <v-card-text class="text">D: {{result.totalD}}点</v-card-text>
        <v-card-text class="text">合計: {{result.total}}点</v-card-text>
        <v-card-action>
          <v-btn to="/" class="primary" >ホームページ</v-btn>
        </v-card-action>
      </v-col>
    </v-row>
  </v-card>
</template>

<script>
export default {
  name: 'ResultForm',
  props: {
    result: {
      type: Object,
      default: null
    }
  },
  data () {
    return {
    }
  }
}
</script>
