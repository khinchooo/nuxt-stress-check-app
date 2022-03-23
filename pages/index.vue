<template>
<div>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <login-form v-if="!$store.getters.isAuthenticated" />
      <div v-else align="center">
        <base-form /><br>
        <v-btn to="/start" class="primary">ストレスチェックを開始</v-btn>
      </div>
    </v-col>
  </v-row>
  <v-row v-if="results.length !== 0">
    <v-col>
      <v-card class="overflow-y-auto mx-auto" max-height="400" max-width="500">
        <h4 align="center"> 過去のストレスチェック結果 </h4>
        <v-divider />
        <v-list dense color="#FCE4EC">
          <v-list-item v-for="res in results" :key="res.id">
            <v-list-item-content>
              {{ res.checkDate ? $dateFns.format(res.checkDate.toDate(), 'yyyy-MM-dd HH:mm') : '' }}
            </v-list-item-content>
            <v-list-item-content>{{ res.hightStress }}</v-list-item-content>
          </v-list-item>
        </v-list>
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
