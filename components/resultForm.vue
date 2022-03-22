<template>
  <v-card color="transparent" height="300">
    <v-row class="mt-1" align="center" justify="center">
      <v-col cols="12">
        <v-card-text class="text-h6">{{ result.checkDate ? $dateFns.format(result.checkDate.toDate(), 'yyyy/MM/dd HH:mm') : '' }}</v-card-text>
        <v-card-text class="text-h6">ストレス：{{result.hightStress}}</v-card-text>
        <v-card-text class="text">A {{result.totalA}}点</v-card-text>
        <v-card-text class="text">B {{result.totalB}}点</v-card-text>
        <v-card-text class="text">C {{result.totalC}}点</v-card-text>
        <v-card-text class="text">D {{result.totalD}}点</v-card-text>
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
