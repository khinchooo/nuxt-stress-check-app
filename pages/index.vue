<template>
<div>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <login-form v-if="!$store.getters.isAuthenticated" />
      <div v-else>
        <base-form />
        <v-btn to="/start" class="primary">開始</v-btn>
      </div>
    </v-col>
  </v-row>
  <v-row v-if="results && results.length > 0">
  <!-- <v-row> -->
    <v-col>
      <v-card>
        <v-list-item v-for="res in results" :key="res.id">
          <v-card-item>{{ res.hightStress }}</v-card-item>
          <v-card-text>{{ $dateFns.format(res.checkDate.toDate(), 'yyyy/MM/dd HH:mm') }}</v-card-text>
        </v-list-item>
      </v-card>
    </v-col>
  </v-row>
</div>
</template>

<script>
import { collection, query, orderBy, where, onSnapshot } from 'firebase/firestore'
import { db } from '../plugins/firebase'

export default {
  name: 'IndexPage',
  data () {
    return {
      results: []
    }
  },
  computed: {
    user () {
      return this.$store.state.user
    }
  },
  mounted () {
    const resultsCollectionRef = collection(db, 'results')
    const q = query(resultsCollectionRef, where('user_id', '==', this.$store.state.user.uid), orderBy('checkDate', 'desc'))
    onSnapshot(q, (querySnapshot) => {
      this.results = querySnapshot.docs.map(doc => ({ ...doc.data(), id: doc.id }))
    })
  }
}
</script>
